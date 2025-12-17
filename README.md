# wxf_parser
A header-only WXF Encoder/Decoder in C++ 20

# What is WXF

WXF is a binary format for faithfully serializing Wolfram Language expressions
in a form suitable for outside storage or interchange with other programs.
WXF can readily be interpreted using low-level native types available in many
programming languages, making it suitable as a format for reading and writing
Wolfram Language expressions in other programming languages.

The details of the WXF format are described in the Wolfram Language documentation:
https://reference.wolfram.com/language/tutorial/WXFFormatDescription.html.en .

# Decoder

Example:
```
	SparseArray[{{1, 1} -> 1/3.0, {1, 23133} ->
	N[Pi, 100] + I N[E, 100], {44, 2} -> -(4/
	 33333333333333444333333335), {_, _} -> 0}]
```
its FullForm is 
```
	SparseArray[Automatic,List[44,23133],0,
	List[1,List[List[0,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,
	2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,3],
	List[List[1],List[23133],List[2]]],
	List[0.3333333333333333`,
	Complex[3.1415926535897932384626433832795028841971693993751058209
	749445923078164062862089986280348253421170679821480865191976`100.,
	2.7182818284590452353602874713526624977572470936999595749669676277
	240766303535475945713821785251664274274663919320031`100.],
	Rational[-4,33333333333333444333333335]]]]
```
C++ usage:
```cpp
	std::vector<uint8_t> test{ 56, 58, 102, 4, 115, 11, 83, 112, 97, 114, 115, 101, 65, 114, 114, \
							97, 121, 115, 9, 65, 117, 116, 111, 109, 97, 116, 105, 99, 193, 1, 1, \
							2, 44, 0, 93, 90, 67, 0, 102, 3, 115, 4, 76, 105, 115, 116, 67, 1, \
							102, 2, 115, 4, 76, 105, 115, 116, 193, 0, 1, 45, 0, 2, 2, 2, 2, 2, \
							2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, \
							2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 193, 1, 2, 3, 1, 1, \
							0, 93, 90, 2, 0, 102, 3, 115, 4, 76, 105, 115, 116, 114, 85, 85, 85, \
							85, 85, 85, 213, 63, 102, 2, 115, 7, 67, 111, 109, 112, 108, 101, \
							120, 82, 122, 51, 46, 49, 52, 49, 53, 57, 50, 54, 53, 51, 53, 56, 57, \
							55, 57, 51, 50, 51, 56, 52, 54, 50, 54, 52, 51, 51, 56, 51, 50, 55, \
							57, 53, 48, 50, 56, 56, 52, 49, 57, 55, 49, 54, 57, 51, 57, 57, 51, \
							55, 53, 49, 48, 53, 56, 50, 48, 57, 55, 52, 57, 52, 52, 53, 57, 50, \
							51, 48, 55, 56, 49, 54, 52, 48, 54, 50, 56, 54, 50, 48, 56, 57, 57, \
							56, 54, 50, 56, 48, 51, 52, 56, 50, 53, 51, 52, 50, 49, 49, 55, 48, \
							54, 55, 57, 56, 50, 49, 52, 56, 48, 56, 54, 53, 49, 57, 49, 57, 55, \
							54, 96, 49, 48, 48, 46, 82, 122, 50, 46, 55, 49, 56, 50, 56, 49, 56, \
							50, 56, 52, 53, 57, 48, 52, 53, 50, 51, 53, 51, 54, 48, 50, 56, 55, \
							52, 55, 49, 51, 53, 50, 54, 54, 50, 52, 57, 55, 55, 53, 55, 50, 52, \
							55, 48, 57, 51, 54, 57, 57, 57, 53, 57, 53, 55, 52, 57, 54, 54, 57, \
							54, 55, 54, 50, 55, 55, 50, 52, 48, 55, 54, 54, 51, 48, 51, 53, 51, \
							53, 52, 55, 53, 57, 52, 53, 55, 49, 51, 56, 50, 49, 55, 56, 53, 50, \
							53, 49, 54, 54, 52, 50, 55, 52, 50, 55, 52, 54, 54, 51, 57, 49, 57, \
							51, 50, 48, 48, 51, 49, 96, 49, 48, 48, 46, 102, 2, 115, 8, 82, 97, \
							116, 105, 111, 110, 97, 108, 67, 252, 73, 26, 51, 51, 51, 51, 51, 51, \
							51, 51, 51, 51, 51, 51, 51, 51, 52, 52, 52, 51, 51, 51, 51, 51, 51, \
							51, 51, 53 };
	auto tree = WXF_PARSER::make_expr_tree(test);
	for (const auto& token : tree.tokens) {
		token.print();
	};
```
we will get that
```
	func: 4 vars
	symbol: SparseArray
	symbol: Automatic
	array: rank = 1, dimensions = 2
	data: 44 23133
	i8: 0
	func: 3 vars
	symbol: List
	i8: 1
	func: 2 vars
	symbol: List
	array: rank = 1, dimensions = 45
	data: 0 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 3
	array: rank = 2, dimensions = 3 1
	data: 1 23133 2
	func: 3 vars
	symbol: List
	f64: 0.333333
	func: 2 vars
	symbol: Complex
	bigreal: 3.1415926535897932384626433832795028841971693993751058209749445923078164062862089986280348253421170679821480865191976`100.
	bigreal: 2.7182818284590452353602874713526624977572470936999595749669676277240766303535475945713821785251664274274663919320031`100.
	func: 2 vars
	symbol: Rational
	i8: -4
	bigint: 33333333333333444333333335
```
Just do what you want.

# Encoder

The `WXF_PARSER::Encoder` struct (a `std::vector<uint8_t>` with additional functionality) provides various `push_xxx` methods for encoding. For example,

```cpp
#include "wxf_parser.h"

// SparseArray[Automatic,#dims,0,{1,{#rowptr,#colindex},#vals}]
int main() {
	WXF_PARSER::Encoder encoder;

	// header of WXF
    encoder.push_ustr(std::vector<uint8_t>{56, 58});
	// SparseArray[Automatic,
    encoder.push_function("SparseArray", 4).push_symbol("Automatic");
    // dims, 0, 
    encoder.push_packed_array({ 2 }, std::vector<int64_t>({ 4,5 })).push_integer(0);
    // {1,
    encoder.push_function("List", 3).push_integer(1);
	// {rowptr
    encoder.push_function("List", 2).push_packed_array({ 5 }, std::vector<int8_t>({ 0,2,4,4,7 }));
	// colindex}
    encoder.push_packed_array({ 7,1 }, std::vector<int16_t>({ 1,3,2,4,1,3,5 }));
    // vals}]
    encoder.push_packed_array({ 7 }, std::vector<double>({ 1.0,2.0,3.0,4.0,5.0,6.0,7.0 }));

    // ....
}
```

We also support template-based encoding as:
```cpp
#include "wxf_parser.h"

int main() {
	std::string_view ff_template = "SparseArray[Automatic,#dims,0,{1,{#rowptr,#colindex},#vals}]";

	std::unordered_map<std::string, WXF_PARSER::Encoder> func_map;

	func_map["#dims"] = WXF_PARSER::Encoder().push_packed_array({ 2 }, std::vector<int64_t>({ 4,5 }));
	func_map["#rowptr"] = WXF_PARSER::Encoder().push_packed_array({ 5 }, std::vector<int8_t>({ 0,2,4,4,7 }));
	func_map["#colindex"] = WXF_PARSER::Encoder().push_packed_array({ 7,1 }, std::vector<int16_t>({ 1,3,2,4,1,3,5 }));
	func_map["#vals"] = WXF_PARSER::Encoder().push_packed_array({ 7 }, std::vector<double>({ 1.0,2.0,3.0,4.0,5.0,6.0,7.0 }));

	auto encoder = WXF_PARSER::fullform_to_wxf(ff_template, func_map);

    // ...
}
```
It is more readable. In templates, we use `#xxxx` to mark sub-expressions, which can be mapped to:

1. A `WXF_PARSER::Encoder`

2. A function `std::function<void(WXF_PARSER::Encoder&)>` for more complex cases

Template placeholders must use only characters: `[0-9,a-z,A-Z,$,{,},[,]]`

One can turn off the WXF head `{56,58}` in 
`WXF_PARSER::fullform_to_wxf` by setting 
`WXF_PARSER::fullform_to_wxf(ff_template, func_map, false)`, which
allows us to use it not only for the global expression, i.e., 
we can use templates to generate some `#xxx` in the above level.

They both give `encoder.buffer` as a `std::vector<uint8_t>` as 
```
tmp = {56, 58, 102, 4, 115, 11, 83, 112, 97, 114, 115, 101, 65, 114, 114, \
97, 121, 115, 9, 65, 117, 116, 111, 109, 97, 116, 105, 99, 193, 3, 1, \
2, 4, 0, 0, 0, 0, 0, 0, 0, 5, 0, 0, 0, 0, 0, 0, 0, 67, 0, 102, 3, \
115, 4, 76, 105, 115, 116, 67, 1, 102, 2, 115, 4, 76, 105, 115, 116, \
193, 0, 1, 5, 0, 2, 4, 4, 7, 193, 1, 2, 7, 1, 1, 0, 3, 0, 2, 0, 4, 0, \
1, 0, 3, 0, 5, 0, 193, 35, 1, 7, 0, 0, 0, 0, 0, 0, 240, 63, 0, 0, 0, \
0, 0, 0, 0, 64, 0, 0, 0, 0, 0, 0, 8, 64, 0, 0, 0, 0, 0, 0, 16, 64, 0, \
0, 0, 0, 0, 0, 20, 64, 0, 0, 0, 0, 0, 0, 24, 64, 0, 0, 0, 0, 0, 0, \
28, 64};
```
and in Mathematica, and `Normal@BinaryDeserialize@ByteArray[tmp]` gives
```
{{1., 0, 2., 0, 0}, {0, 3., 0, 4., 0}, {0, 0, 0, 0, 0}, {5., 0, 6., 0, 7.}}.
```

This parser processes FullForm expressions directly. 
There is no support for infix operators like `_,/, *, ^, %, ...`. 
Use `Plus[1,1]` instead of `1+1`, `Rule[a, b]` instead of `a->b`, for example.
Since List is widely used, we automatically convert `{ }` to `List[ ]`.

## Limitations

* High precision numbers and Large integers in templates: Not supported; only standard C++ numeric formats are available. Just define a `#a` and use `push_bigint` or `push_bigreal` for it.

* Performance: For complex expressions, manually handle sub-expressions using `push_xxx` methods in Encoder for better performance.

* Robustness: Keep templates simple and handle complicated sub-expressions manually.
