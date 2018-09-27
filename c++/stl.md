# STL

## vector

## bitset

bitset 不是 STL 容器类, 不能调整大小.

bitset 初始化:

```c++
#include <bitset>
using namespace std;

int main(void) {
  bitset <4> fourbits;
  bitset <5> fivebits("10100");
  bitset <8> eightbits(255);
  bitset <8> cpbits(eightbits);
  return 0;
}
```

bitset 运算符

| 运算符 | 描述                                        |
| ------ | ------------------------------------------- |
| <<     | 将字节的字符表示插入到输出流                |
| >>     | 将字节的字符表示插入到 bitset 对象中        |
| &      | 按字节与操作                                |
| \|     | 按字节或操作                                |
| ^      | 按字节异或操作 res = fourbits ^ fourbits2   |
| ~      | 按字节取反操作 res = ~fourbits              |
| >>=    | 按字节右移操作 fourbits >>= (2)             |
| <<=    | 按字节左移操作 fourbits <<= (2)             |
| [N]    | 指向位序列中第 N+1 位的应用 fourbits[2] = 0 |

|=, &=, ^=, ~= 等运算符, 可用于对 bitset 对象执行按位操作.
