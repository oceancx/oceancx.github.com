---
---

2.3 复合类型 (Compund type)
	2.3.1 引用
	2.3.2 指针
	2.3.3 理解复合类型的声明

2.4 const限定符
	2.4.1 const引用
	2.4.2 指针和const

6.2 参数传递
	6.2.2 传引用参数
	
		void reset(int &i){i = 0;}

		int j=42;
		reset(j);
		cout<< "j = "<<j<<endl;  // j = 0


	引用可以作为参数 返回一些额外的信息

	
