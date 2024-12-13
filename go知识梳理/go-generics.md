# Go 泛型使用指南

## 1. 基础语法

### 1.1 类型参数声明
```go
// 基本语法
func Print[T any](slice []T) {
    for _, v := range slice {
        fmt.Print(v, " ")
    }
}

// 多个类型参数
func Map[T, U any](slice []T, f func(T) U) []U {
    result := make([]U, len(slice))
    for i, v := range slice {
        result[i] = f(v)
    }
    return result
}
```

### 1.2 类型约束
```go
// 使用接口约束
type Number interface {
    ~int | ~int32 | ~int64 | ~float32 | ~float64
}

func Sum[T Number](numbers []T) T {
    var sum T
    for _, v := range numbers {
        sum += v
    }
    return sum
}

// 使用 comparable
func Contains[T comparable](slice []T, elem T) bool {
    for _, v := range slice {
        if v == elem {
            return true
        }
    }
    return false
}
```

## 2. 实用示例

### 2.1 泛型数据结构
```go
// 泛型栈
type Stack[T any] struct {
    items []T
}

func (s *Stack[T]) Push(item T) {
    s.items = append(s.items, item)
}

func (s *Stack[T]) Pop() (T, error) {
    var zero T
    if len(s.items) == 0 {
        return zero, errors.New("stack is empty")
    }
    item := s.items[len(s.items)-1]
    s.items = s.items[:len(s.items)-1]
    return item, nil
}

// 泛型链表节点
type Node[T any] struct {
    Value T
    Next  *Node[T]
}

// 泛型二叉树
type TreeNode[T any] struct {
    Value T
    Left  *TreeNode[T]
    Right *TreeNode[T]
}
```

### 2.2 泛型算法
```go
// 泛型排序
func Sort[T constraints.Ordered](slice []T) {
    sort.Slice(slice, func(i, j int) bool {
        return slice[i] < slice[j]
    })
}

// 泛型二分查找
func BinarySearch[T constraints.Ordered](slice []T, target T) int {
    left, right := 0, len(slice)-1
    for left <= right {
        mid := (left + right) / 2
        if slice[mid] == target {
            return mid
        } else if slice[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return -1
}
```

## 3. 高级用法

### 3.1 类型集合
```go
// 自定义类型集合
type Numeric interface {
    ~int | ~int8 | ~int16 | ~int32 | ~int64 |
    ~uint | ~uint8 | ~uint16 | ~uint32 | ~uint64 |
    ~float32 | ~float64
}

// 带方法的类型约束
type Stringer interface {
    String() string
}

type Printable interface {
    ~string | Stringer
}

func PrintValue[T Printable](v T) {
    fmt.Println(v)
}
```

### 3.2 泛型容器
```go
// 泛型 Set
type Set[T comparable] struct {
    items map[T]struct{}
}

func NewSet[T comparable]() *Set[T] {
    return &Set[T]{
        items: make(map[T]struct{}),
    }
}

func (s *Set[T]) Add(item T) {
    s.items[item] = struct{}{}
}

func (s *Set[T]) Remove(item T) {
    delete(s.items, item)
}

func (s *Set[T]) Contains(item T) bool {
    _, exists := s.items[item]
    return exists
}

// 泛型 Queue
type Queue[T any] struct {
    items []T
}

func (q *Queue[T]) Enqueue(item T) {
    q.items = append(q.items, item)
}

func (q *Queue[T]) Dequeue() (T, error) {
    var zero T
    if len(q.items) == 0 {
        return zero, errors.New("queue is empty")
    }
    item := q.items[0]
    q.items = q.items[1:]
    return item, nil
}
```

## 4. 实践建议

### 4.1 性能考虑
```go
// 避免过度使用泛型
// 不好的例子
func WrapValue[T any](v T) T {
    return v
}

// 好的例子
func ProcessNumbers[T Number](numbers []T) []T {
    result := make([]T, len(numbers))
    for i, v := range numbers {
        result[i] = v * 2
    }
    return result
}
```

### 4.2 最佳实践
```go
// 使用有意义的类型约束
type JSONMarshaler interface {
    MarshalJSON() ([]byte, error)
}

func EncodeToJSON[T JSONMarshaler](items []T) ([]byte, error) {
    result := make([]map[string]interface{}, len(items))
    for i, item := range items {
        data, err := item.MarshalJSON()
        if err != nil {
            return nil, err
        }
        var m map[string]interface{}
        if err := json.Unmarshal(data, &m); err != nil {
            return nil, err
        }
        result[i] = m
    }
    return json.Marshal(result)
}

// 使用类型参数提高代码复用
type Result[T any] struct {
    Data    T
    Error   error
    Success bool
}

func NewResult[T any](data T, err error) Result[T] {
    return Result[T]{
        Data:    data,
        Error:   err,
        Success: err == nil,
    }
}
```

## 5. 常见错误处理

### 5.1 编译错误
```go
// 错误：类型参数不满足约束
func Process[T string](v T) { // 编译错误：string 不是有效的约束
    // ...
}

// 正确：使用适当的约束
func Process[T ~string](v T) {
    // ...
}

// 错误：无法推断类型参数
var s = NewSet() // 编译错误：无法推断类型参数

// 正确：明确指定类型参数
var s = NewSet[string]()
```

### 5.2 运行时注意事项
```go
// 注意：泛型和反射
func PrintType[T any](v T) {
    fmt.Printf("Type: %T\n", v) // 可能不会显示预期的类型信息
}

// 建议：使用类型断言时要小��
func Convert[T any](v interface{}) (T, error) {
    if result, ok := v.(T); ok {
        return result, nil
    }
    var zero T
    return zero, errors.New("type conversion failed")
}
``` 