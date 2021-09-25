选用 
00000000004019ae <setval_237>:
  4019ae:	c7 07 48 89 c7 c7    	movl   $0xc7c78948,(%rdi)
  4019b4:	c3                   	retq 
报错，可能是因为最后的 c7 影响了 48 88 c7 的movq指令
