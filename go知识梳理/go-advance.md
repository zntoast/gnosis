# Go 语言进阶指南

## 1. 并发编程

### 1.1 Goroutine 最佳实践
- 优雅退出
```go
func worker(ctx context.Context) {
    for {
        select {
        case <-ctx.Done():
            return
        default:
            // 执行业务逻辑
        }
    }
}
```

- 控制并发数
```go
func main() {
    sem := make(chan struct{}, 3) // 最多3个并发
    for i := 0; i < 10; i++ {
        sem <- struct{}{} // 获取信号量
        go func(id int) {
            defer func() { <-sem }() // 释放信号量
            // 执行任务
        }(i)
    }
}
```

### 1.2 Channel 使用技巧
- 单向通道
```go
func produce(ch chan<- int) {
    for i := 0; i < 10; i++ {
        ch <- i
    }
    close(ch)
}

func consume(ch <-chan int) {
    for v := range ch {
        fmt.Println(v)
    }
}
```

- 超时控制
```go
select {
case data := <-ch:
    // 处理数据
case <-time.After(time.Second * 3):
    // 超时处理
}
```

## 2. 内存管理

### 2.1 内存逃逸
```go
// 会发生逃逸
func getPointer() *int {
    v := 42
    return &v
}

// 不会发生逃逸
func getValue() int {
    v := 42
    return v
}
```

### 2.2 内存优化
- 使用对象池
```go
var pool = sync.Pool{
    New: func() interface{} {
        return make([]byte, 1024)
    },
}

func process() {
    buf := pool.Get().([]byte)
    defer pool.Put(buf)
    // 使用 buf
}
```

## 3. 错误处理

### 3.1 错误包装
```go
import "github.com/pkg/errors"

func readConfig() error {
    err := readFile()
    if err != nil {
        return errors.Wrap(err, "failed to read config")
    }
    return nil
}
```

### 3.2 自定义错误
```go
type ValidationError struct {
    Field string
    Error string
}

func (v *ValidationError) Error() string {
    return fmt.Sprintf("validation failed on %s: %s", v.Field, v.Error)
}
```

## 4. 性能优化

### 4.1 内存分配
- 预分配内存
```go
// 不好的做法
s := make([]int, 0)
for i := 0; i < 10000; i++ {
    s = append(s, i)
}

// 好的做法
s := make([]int, 0, 10000)
for i := 0; i < 10000; i++ {
    s = append(s, i)
}
```

### 4.2 字符串处理
```go
// 不好的做法
s := ""
for i := 0; i < 1000; i++ {
    s += "a"
}

// 好的做法
var builder strings.Builder
for i := 0; i < 1000; i++ {
    builder.WriteString("a")
}
s := builder.String()
```

## 5. 测试技巧

### 5.1 表驱动测试
```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"positive", 2, 3, 5},
        {"negative", -2, -3, -5},
        {"zero", 0, 0, 0},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            if got := Add(tt.a, tt.b); got != tt.expected {
                t.Errorf("Add() = %v, want %v", got, tt.expected)
            }
        })
    }
}
```

### 5.2 性能测试
```go
func BenchmarkConcat(b *testing.B) {
    b.ReportAllocs()
    for i := 0; i < b.N; i++ {
        var builder strings.Builder
        for j := 0; j < 100; j++ {
            builder.WriteString("a")
        }
        _ = builder.String()
    }
}
```

## 6. 工程实践

### 6.1 项目结构
```
project/
├── cmd/                    # 主程序入口
├── internal/               # 私有代码
│   ├── pkg/               # 内部包
│   └── app/               # 应用逻辑
├── pkg/                    # 公共包
├── api/                    # API定义
├── configs/                # 配置文件
├── scripts/                # 脚本
└── test/                   # 测试
```

### 6.2 依赖注入
```go
type Service struct {
    repo   Repository
    cache  Cache
    logger Logger
}

func NewService(repo Repository, cache Cache, logger Logger) *Service {
    return &Service{
        repo:   repo,
        cache:  cache,
        logger: logger,
    }
}
```

## 7. 微服务设计

### 7.1 服务发现
```go
func registerService() {
    consul, err := api.NewClient(api.DefaultConfig())
    if err != nil {
        log.Fatal(err)
    }

    registration := &api.AgentServiceRegistration{
        ID:      "service-1",
        Name:    "my-service",
        Port:    8080,
        Address: "127.0.0.1",
    }

    if err := consul.Agent().ServiceRegister(registration); err != nil {
        log.Fatal(err)
    }
}
```

### 7.2 熔断器
```go
import "github.com/sony/gobreaker"

var cb *gobreaker.CircuitBreaker

func init() {
    cb = gobreaker.NewCircuitBreaker(gobreaker.Settings{
        Name:        "my-service",
        MaxRequests: 3,
        Interval:    10 * time.Second,
        Timeout:     60 * time.Second,
    })
}

func callService() error {
    result, err := cb.Execute(func() (interface{}, error) {
        // 调用服务
        return nil, nil
    })
    return err
}
```

## 8. 安全编程

### 8.1 密码加密
```go
func hashPassword(password string) (string, error) {
    bytes, err := bcrypt.GenerateFromPassword([]byte(password), 14)
    return string(bytes), err
}

func checkPassword(password, hash string) bool {
    err := bcrypt.CompareHashAndPassword([]byte(hash), []byte(password))
    return err == nil
}
```

### 8.2 SQL 注入防护
```go
// 不安全的方式
db.Query("SELECT * FROM users WHERE name = '" + name + "'")

// 安全的方式
db.Query("SELECT * FROM users WHERE name = ?", name)
```

## 9. 调试和监控

### 9.1 pprof 使用
```go
import _ "net/http/pprof"

func main() {
    go func() {
        log.Println(http.ListenAndServe("localhost:6060", nil))
    }()
    // ... 主程序逻辑
}
```

### 9.2 Prometheus 监控
```go
import "github.com/prometheus/client_golang/prometheus"

var (
    requestsTotal = prometheus.NewCounterVec(
        prometheus.CounterOpts{
            Name: "http_requests_total",
            Help: "Total number of HTTP requests",
        },
        []string{"method", "endpoint"},
    )
)

func init() {
    prometheus.MustRegister(requestsTotal)
}
```

