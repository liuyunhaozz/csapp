使用了 `farm.c` 中的garget
```
00000000004019c3 <setval_426>:
  4019c3:	c7 07 48 89 c7 90    	movl   $0x90c78948,(%rdi)
  4019c9:	c3                   	retq   

``` 
从字节 `48` 处开始执行，事实证明字节 `90` 不会影响前面的字节，可能是因为在此时字节 `90` 被解释为 `nop` 指令的缘故