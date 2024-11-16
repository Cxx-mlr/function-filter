# Function-Filter
    by: konanTheBarbar & Cxx
```cpp
#define _CMP_BEGIN namespace cmp {
#define _CMP_END }

_CMP_BEGIN
template <class T>
class CompareImpl
{
  public:
        constexpr CompareImpl(T f) : func(f) {}

	template <class X0>
	constexpr decltype(auto) operator< (X0 val)
	{
		return [func_ = func, val](auto... args) {
			return func_(args...) < val;
		};
	}

	template <class X0>
	constexpr decltype(auto) operator> (X0 val)
	{
		return [func_ = func, val](auto... args) {
			return func_(args...) > val;
		};
	}

	template <class X0>
	constexpr decltype(auto) operator<= (X0 val)
	{
		return [func_ = func, val](auto... args) {
			return func_(args...) <= val;
		};
	}

	template <class X0>
	constexpr decltype(auto) operator>= (X0 val)
	{
		return [func_ = func, val](auto... args) {
			return func_(args...) >= val;
		};
	}

	template <class X0>
	constexpr decltype(auto) operator== (X0 val)
	{
		return [func_ = func, val](auto... args) {
			return func_(args...) == val;
		};
	}

	template <class X0>
	constexpr decltype(auto) operator!= (X0 val)
	{
		return [func_ = func, val](auto... args) {
			return func_(args...) != val;
		};
	}

  private:
	T func;
};

template<typename T>
constexpr auto Compare(T t = {})
{
	return CompareImpl<T>(t);
}

template<auto FuncRef>
constexpr auto Compare()
{
	return CompareImpl<decltype(FuncRef)>(FuncRef);
}

template <class X, class Y>
constexpr decltype(auto) operator&& (X lhs, Y rhs) {
	return [lhs, rhs](auto... args) {
		return lhs(args...) && rhs(args...);
	};
}

template <class X, class Y>
constexpr decltype(auto) operator|| (X lhs, Y rhs) {
	return [lhs, rhs](auto... args) {
		return lhs(args...) || rhs(args...);
	};
}

template <class X>
constexpr decltype(auto) operator!(X lhs) {
	return [lhs](auto... args) {
		return !lhs(args...);
	};
}
_CMP_END

constexpr int doubleFunc(int someArg)
{
	return someArg * 2;
}

using namespace cmp;

int main() {
	constexpr auto newFilter = cmp::Compare<doubleFunc>() >= 0 && cmp::Compare(doubleFunc) < 20;
	constexpr bool isSmallerThan20 = newFilter(5);
  
	return isSmallerThan20 ? 42 : 0;
}
```

1: KonanTheBarbar - https://godbolt.org/z/M3p3-t \
2: Cxx - https://godbolt.org/z/ki55Qx \
3: KonanTheBarbar - https://godbolt.org/z/ZzKTw0

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
