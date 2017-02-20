在`json11::Json`类的内部，使用JsonValue类来表示Json对象所需要的类型。（这句翻译的不好，欢迎提供更好的翻译）


 有关numbers，这里要特别说明一下
 
JSON在语法上区分整数和浮点数，但是在语义上不做区分。所以，有的JSON实现区分整数和浮点数，有些则不区分。

在json11中，我们不区分整数和浮点数。因为在一些JSON的实现中（比如Javascript），把数字都看作是同一个类型，交给JSON去处理，导致数字被`round-trip`隐式转换。

这是非常危险的! 为了规避这个风险，json11将数字都保存为double类型，同时提供一些整数的辅助方法。

double的精度IEEE754 ('double')可以精确的保存 `+/-2^53`范围内的整数,幸运地是，大多数系统的'int'都在这个范围内。(时间戳通常使用 int64或者long long来避免Y2038K问题；a double storing microseconds since some epoch will be exact for `+/- 275` years.

（最后这句啥意思没看懂）

```c++
#pragma once
```

这个宏相当于头文件保护？完全等价吗？

```c++
#ifdef _MSC_VER  
    #if _MSC_VER <= 1800 // VS 2013  
        #ifndef noexcept  
            #define noexcept throw()  
        #endif  

        #ifndef snprintf  
            #define snprintf _snprintf_s  
        #endif  
    #endif  
#endif  
```

这里的define有什么用途？

```C++
class Json final {  
```

final是什么用途？

```c++
Json() noexcept;  
```

函数后面加noexcept什么语法？
    
```c++
Json(std::nullptr_t) noexcept;  
```

这个`std::nullptr_t`是什么东西？  
为什么使用这个类型？
    
```c++
Json(std::string &&value);  
Json(array &&values);  
Json(object &&values);  
```

`&&`是什么意思？
    

```c++
template <class T, class = decltype(&T::to_json)>  
Json(const T & t) : Json(t.to_json()) {}  
```

模板类的定义，要看一下书

```c++
// Implicit constructor: map-like objects (std::map, std::unordered_map, etc)  
template <class M, typename std::enable_if<
    std::is_constructible<std::string, typename M::key_type>::value
    && std::is_constructible<Json, typename M::mapped_type>::value,
        int>::type = 0>  
Json(const M & m) : Json(object(m.begin(), m.end())) {}           
```

注释说，只要传入一个`map-like`的对象就可以，这是怎么做到的？

    
```c++
// Implicit constructor: vector-like objects (std::list, std::vector, std::set, etc)  
template <class V, typename std::enable_if<
    std::is_constructible<Json, typename V::value_type>::value,
        int>::type = 0>  
Json(const V & v) : Json(array(v.begin(), v.end())) {}  
```

同上，`vector-like`

```c++
// This prevents Json(some_pointer) from accidentally producing a bool. Use  
// Json(bool(some_pointer)) if that behavior is desired.  
Json(void *) = delete;  
```
    
啥意思？

```c++
void dump(std::string &out) const;  
std::string dump() const {  
    std::string out;  
    dump(out);  
    return out;  
}  
```

提供了两种不同的结果返回形式

```c++
const char * in;  
std::string(in);  
```

不大记得了，string的其他构造？


```c++
static inline std::vector<Json> parse_multi(  
    const std::string & in,  
    std::string & err,  
    JsonParse strategy = JsonParse::STANDARD) {  
    std::string::size_type parser_stop_pos;  
    return parse_multi(in, parser_stop_pos, err, strategy);  
}  
```

`std::string::size_type parser_stop_pos;`默认初始化得到的值是多少？

```c++
bool operator== (const Json &rhs) const;  
bool operator<  (const Json &rhs) const;  
bool operator!= (const Json &rhs) const { return !(*this == rhs); }  
bool operator<= (const Json &rhs) const { return !(rhs < *this); }  
bool operator>  (const Json &rhs) const { return  (rhs < *this); }  
bool operator>= (const Json &rhs) const { return !(*this < rhs); }  
```

比较运算符只需要实现`==`和`<`就可以了，其他的通过这两个得到？

```c++
private:  
    std::shared_ptr<JsonValue> m_ptr;  
```

你知道把数据都包装在JsonValue类中的好处吗？

```c++
// Internal class hierarchy - JsonValue objects are not exposed to users of this API.  
```

这里是其中的一个好处？没有暴露给用户。

```c++

class JsonValue {  
protected:  
    friend class Json;  
    friend class JsonInt;  
    friend class JsonDouble;  
    virtual Json::Type type() const = 0;  
    virtual bool equals(const JsonValue * other) const = 0;  
    virtual bool less(const JsonValue * other) const = 0;  
    virtual void dump(std::string &out) const = 0;  
    virtual double number_value() const;  
    virtual int int_value() const;  
    virtual bool bool_value() const;  
    virtual const std::string &string_value() const;  
    virtual const Json::array &array_items() const;  
    virtual const Json &operator[](size_t i) const;  
    virtual const Json::object &object_items() const;  
    virtual const Json &operator[](const std::string &key) const;  
    virtual ~JsonValue() {}  
};  
```

核心数据类，需要多看
