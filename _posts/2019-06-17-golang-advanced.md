---
layout: post
title: "Go语言高级使用总结"
date: 2019-06-17
categories: [golang, programming]
tags: [golang, concurrency, performance, advanced]
---

# Go 语言高级使用总结

## 1. 并发编程 (Concurrency)

### 1.1 Goroutines 高级用法

```go
// 使用 sync.WaitGroup 管理多个 goroutine
func processItems(items []Item) {
    var wg sync.WaitGroup
    results := make(chan Result, len(items))

    for _, item := range items {
        wg.Add(1)
        go func(i Item) {
            defer wg.Done()
            results <- process(i)
        }(item)
    }

    // 等待所有 goroutine 完成
    go func() {
        wg.Wait()
        close(results)
    }()

    // 收集结果
    for result := range results {
        handleResult(result)
    }
}
```

### 1.2 Channel 模式

**Worker Pool 模式**

```go
func workerPool(jobs <-chan Job, results chan<- Result, numWorkers int) {
    var wg sync.WaitGroup

    for i := 0; i < numWorkers; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            for job := range jobs {
                results <- processJob(job)
            }
        }()
    }

    wg.Wait()
    close(results)
}
```

**Pipeline 模式**

```go
func pipeline() {
    // 阶段 1: 生成数据
    gen := func(nums ...int) <-chan int {
        out := make(chan int)
        go func() {
            defer close(out)
            for _, n := range nums {
                out <- n
            }
        }()
        return out
    }

    // 阶段 2: 处理数据
    square := func(in <-chan int) <-chan int {
        out := make(chan int)
        go func() {
            defer close(out)
            for n := range in {
                out <- n * n
            }
        }()
        return out
    }

    // 组装 pipeline
    c := gen(2, 3, 4)
    out := square(c)

    for n := range out {
        fmt.Println(n) // 4, 9, 16
    }
}
```

### 1.3 Select 高级技巧

```go
// 超时控制
select {
case result := <-ch:
    return result
case <-time.After(5 * time.Second):
    return fmt.Errorf("timeout")
}

// 非阻塞操作
select {
case msg := <-ch:
    fmt.Println(msg)
default:
    fmt.Println("no message received")
}

// 优雅退出
func worker(done <-chan struct{}) {
    ticker := time.NewTicker(1 * time.Second)
    defer ticker.Stop()

    for {
        select {
        case <-ticker.C:
            doWork()
        case <-done:
            return
        }
    }
}
```

## 2. Context 包的使用

### 2.1 Context 传递和取消

```go
func operation(ctx context.Context) error {
    // 创建带超时的子 context
    ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
    defer cancel()

    select {
    case <-time.After(3 * time.Second):
        return nil
    case <-ctx.Done():
        return ctx.Err() // context deadline exceeded
    }
}

// 传递请求级别的值
func handleRequest(ctx context.Context) {
    requestID := ctx.Value("requestID").(string)
    log.Printf("Processing request: %s", requestID)
}
```

### 2.2 Context 最佳实践

```go
// 1. 总是将 context 作为第一个参数
func DoSomething(ctx context.Context, arg string) error {
    // ...
}

// 2. 不要在结构体中存储 context
type Service struct {
    // 错误: ctx context.Context
}

// 3. 使用 context.WithValue 传递请求范围的数据
type contextKey string

const requestIDKey contextKey = "requestID"

func setRequestID(ctx context.Context, id string) context.Context {
    return context.WithValue(ctx, requestIDKey, id)
}

func getRequestID(ctx context.Context) string {
    if id, ok := ctx.Value(requestIDKey).(string); ok {
        return id
    }
    return ""
}
```

## 3. 接口的高级使用

### 3.1 接口组合

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}

type ReadWriter interface {
    Reader
    Writer
}

// 空接口的类型断言和类型开关
func process(i interface{}) {
    switch v := i.(type) {
    case int:
        fmt.Printf("Integer: %d\n", v)
    case string:
        fmt.Printf("String: %s\n", v)
    case MyStruct:
        fmt.Printf("MyStruct: %+v\n", v)
    default:
        fmt.Printf("Unknown type: %T\n", v)
    }
}
```

### 3.2 接口最佳实践

```go
// 接口隔离原则: 小而精的接口
type Stringer interface {
    String() string
}

// 依赖接口而不是具体实现
type DataStore interface {
    Save(data []byte) error
    Load() ([]byte, error)
}

type Service struct {
    store DataStore // 而不是具体的 *FileStore
}

// 接口的隐式实现
type MockStore struct{}

func (m *MockStore) Save(data []byte) error { return nil }
func (m *MockStore) Load() ([]byte, error)  { return nil, nil }
// MockStore 自动实现了 DataStore 接口
```

## 4. 反射 (Reflection)

### 4.1 基本用法

```go
import "reflect"

func inspectValue(v interface{}) {
    val := reflect.ValueOf(v)
    typ := reflect.TypeOf(v)

    fmt.Printf("Type: %s\n", typ)
    fmt.Printf("Kind: %s\n", val.Kind())
    fmt.Printf("Value: %v\n", val)
}

// 修改值 (需要传递指针)
func modifyValue(v interface{}) {
    val := reflect.ValueOf(v)
    if val.Kind() == reflect.Ptr && val.Elem().CanSet() {
        val.Elem().SetInt(42)
    }
}
```

### 4.2 结构体反射

```go
type User struct {
    Name  string `json:"name" validate:"required"`
    Email string `json:"email" validate:"email"`
    Age   int    `json:"age" validate:"min=0,max=120"`
}

func inspectStruct(s interface{}) {
    val := reflect.ValueOf(s)
    typ := val.Type()

    for i := 0; i < val.NumField(); i++ {
        field := typ.Field(i)
        value := val.Field(i)

        fmt.Printf("Field: %s, Type: %s, Value: %v\n",
            field.Name, field.Type, value)

        // 获取标签
        if tag := field.Tag.Get("json"); tag != "" {
            fmt.Printf("JSON tag: %s\n", tag)
        }
    }
}

// 调用方法
func callMethod(obj interface{}, methodName string, args ...interface{}) {
    val := reflect.ValueOf(obj)
    method := val.MethodByName(methodName)

    if method.IsValid() {
        in := make([]reflect.Value, len(args))
        for i, arg := range args {
            in[i] = reflect.ValueOf(arg)
        }
        method.Call(in)
    }
}
```

## 5. 泛型 (Generics) - Go 1.18+

### 5.1 泛型函数

```go
// 泛型函数
func Map[T, U any](slice []T, fn func(T) U) []U {
    result := make([]U, len(slice))
    for i, v := range slice {
        result[i] = fn(v)
    }
    return result
}

// 使用约束
func Min[T constraints.Ordered](a, b T) T {
    if a < b {
        return a
    }
    return b
}

// 自定义约束
type Number interface {
    ~int | ~int64 | ~float64
}

func Sum[T Number](numbers []T) T {
    var sum T
    for _, n := range numbers {
        sum += n
    }
    return sum
}
```

### 5.2 泛型类型

```go
// 泛型结构体
type Stack[T any] struct {
    items []T
}

func (s *Stack[T]) Push(item T) {
    s.items = append(s.items, item)
}

func (s *Stack[T]) Pop() (T, bool) {
    if len(s.items) == 0 {
        var zero T
        return zero, false
    }
    item := s.items[len(s.items)-1]
    s.items = s.items[:len(s.items)-1]
    return item, true
}

// 使用
stack := Stack[int]{}
stack.Push(1)
stack.Push(2)
value, ok := stack.Pop() // value = 2, ok = true
```

## 6. 内存管理和性能优化

### 6.1 内存分配优化

```go
// 预分配切片容量
func processLargeData(n int) []Result {
    results := make([]Result, 0, n) // 预分配容量
    for i := 0; i < n; i++ {
        results = append(results, process(i))
    }
    return results
}

// 使用 sync.Pool 重用对象
var bufferPool = sync.Pool{
    New: func() interface{} {
        return new(bytes.Buffer)
    },
}

func processRequest() {
    buf := bufferPool.Get().(*bytes.Buffer)
    defer func() {
        buf.Reset()
        bufferPool.Put(buf)
    }()

    buf.WriteString("data")
    // 处理 buffer
}
```

### 6.2 性能分析

```go
import (
    "runtime/pprof"
    _ "net/http/pprof" // 自动注册 pprof handlers
)

// CPU 性能分析
func profileCPU() {
    f, _ := os.Create("cpu.prof")
    pprof.StartCPUProfile(f)
    defer pprof.StopCPUProfile()

    // 执行需要分析的代码
}

// 内存性能分析
func profileMemory() {
    f, _ := os.Create("mem.prof")
    defer f.Close()

    runtime.GC()
    pprof.WriteHeapProfile(f)
}

// 使用命令分析:
// go tool pprof cpu.prof
// go tool pprof -http=:8080 mem.prof
```

### 6.3 基准测试

```go
// benchmark_test.go
func BenchmarkStringConcat(b *testing.B) {
    for i := 0; i < b.N; i++ {
        var s string
        for j := 0; j < 100; j++ {
            s += "a"
        }
    }
}

func BenchmarkStringBuilder(b *testing.B) {
    for i := 0; i < b.N; i++ {
        var sb strings.Builder
        for j := 0; j < 100; j++ {
            sb.WriteString("a")
        }
        _ = sb.String()
    }
}

// 运行: go test -bench=. -benchmem
```

## 7. 错误处理最佳实践

### 7.1 自定义错误类型

```go
type ValidationError struct {
    Field string
    Err   error
}

func (e *ValidationError) Error() string {
    return fmt.Sprintf("validation failed for field %s: %v", e.Field, e.Err)
}

func (e *ValidationError) Unwrap() error {
    return e.Err
}

// 错误包装 (Go 1.13+)
func process() error {
    if err := validate(); err != nil {
        return fmt.Errorf("process failed: %w", err)
    }
    return nil
}

// 错误检查
if err := process(); err != nil {
    if errors.Is(err, ErrNotFound) {
        // 处理特定错误
    }

    var validationErr *ValidationError
    if errors.As(err, &validationErr) {
        // 处理验证错误
    }
}
```

### 7.2 错误哨兵值

```go
var (
    ErrNotFound     = errors.New("not found")
    ErrUnauthorized = errors.New("unauthorized")
    ErrInvalidInput = errors.New("invalid input")
)

// 使用
func findUser(id string) (*User, error) {
    if user := db.Find(id); user != nil {
        return user, nil
    }
    return nil, ErrNotFound
}
```

## 8. 高级测试技巧

### 8.1 表驱动测试

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"positive numbers", 2, 3, 5},
        {"negative numbers", -2, -3, -5},
        {"mixed", 2, -3, -1},
        {"zeros", 0, 0, 0},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            result := Add(tt.a, tt.b)
            if result != tt.expected {
                t.Errorf("Add(%d, %d) = %d, want %d",
                    tt.a, tt.b, result, tt.expected)
            }
        })
    }
}
```

### 8.2 Mock 和接口测试

```go
type Database interface {
    GetUser(id string) (*User, error)
}

type MockDatabase struct {
    GetUserFunc func(id string) (*User, error)
}

func (m *MockDatabase) GetUser(id string) (*User, error) {
    if m.GetUserFunc != nil {
        return m.GetUserFunc(id)
    }
    return nil, nil
}

func TestService(t *testing.T) {
    mockDB := &MockDatabase{
        GetUserFunc: func(id string) (*User, error) {
            return &User{ID: id, Name: "Test"}, nil
        },
    }

    service := NewService(mockDB)
    user, err := service.GetUser("123")

    if err != nil {
        t.Fatalf("unexpected error: %v", err)
    }
    if user.Name != "Test" {
        t.Errorf("expected name 'Test', got '%s'", user.Name)
    }
}
```

## 9. 构建标签和条件编译

```go
// +build linux darwin
// file_unix.go

package mypackage

func platformSpecific() {
    // Unix-specific implementation
}
```

```go
// +build windows
// file_windows.go

package mypackage

func platformSpecific() {
    // Windows-specific implementation
}
```

## 10. Unsafe 包的使用 (谨慎使用)

```go
import "unsafe"

// 指针转换
func stringToBytes(s string) []byte {
    return *(*[]byte)(unsafe.Pointer(&s))
}

// 获取结构体字段偏移量
type MyStruct struct {
    a int32
    b int64
}

func main() {
    var s MyStruct
    fmt.Println(unsafe.Offsetof(s.b)) // 输出 b 字段的偏移量
}
```

## 总结

Go 语言的高级特性使其成为构建高性能、可扩展系统的理想选择。关键要点：

1. **并发是核心优势**: 善用 goroutines、channels 和 select
2. **接口设计**: 保持小而精，依赖抽象而非具体实现
3. **Context 传递**: 用于超时控制和取消操作
4. **性能优化**: 使用 pprof 分析，预分配内存，复用对象
5. **错误处理**: 返回错误而不是 panic，使用错误包装和检查
6. **泛型**: Go 1.18+ 提供类型安全的泛型编程
7. **测试**: 表驱动测试和接口 mock 使测试更加可靠

掌握这些高级特性，能够编写出更加健壮、高效和可维护的 Go 代码。
