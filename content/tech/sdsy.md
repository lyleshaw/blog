## 3位的二进制优先级编码器

```verilog
module yxj(IN,EN,Y,Done);
input[7:0] IN;
input EN;
output reg Done;
output reg[2:0] Y;
always @(IN or EN)
if(!EN)
    begin
        if(IN[0]) begin Y[2]=0;Y[1]=0;Y[0]=0; Done=0;end
	          else if(IN[1]) begin Y[2]=0;Y[1]=0;Y[0]=1; Done=1;end
	          else if(IN[2]) begin Y[2]=0;Y[1]=1;Y[0]=0; Done=1;end
		          else if(IN[3]) begin Y[2]=0;Y[1]=1;Y[0]=1; Done=1;end
		          else if(IN[4]) begin Y[2]=1;Y[1]=0;Y[0]=0; Done=1;end
		          else if(IN[5]) begin Y[2]=1;Y[1]=0;Y[0]=1; Done=1;end
		          else if(IN[6]) begin Y[2]=1;Y[1]=1;Y[0]=0; Done=1;end
		          else if(IN[7]) begin Y[2]=1;Y[1]=1;Y[0]=1; Done=1;end
		          else begin Y[2]=0;Y[1]=0;Y[0]=0;Done=0;end
	  end
	  else begin Y[2]=0;Y[1]=0;Y[0]=0;Done=0;end
endmodule
```

```verilog
NET "IN[7]" IOSTANDARD = LVCMOS18; NET "IN[7]" LOC = Y6;
NET "IN[6]" IOSTANDARD = LVCMOS18; NET "IN[6]" LOC = Y4;
NET "IN[5]" IOSTANDARD = LVCMOS18; NET "IN[5]" LOC = W4;
NET "IN[4]" IOSTANDARD = LVCMOS18; NET "IN[4]" LOC = V4;
NET "IN[3]" IOSTANDARD = LVCMOS18; NET "IN[3]" LOC = V3;
NET "IN[2]" IOSTANDARD = LVCMOS18; NET "IN[2]" LOC = T4;
NET "IN[1]" IOSTANDARD = LVCMOS18; NET "IN[1]" LOC = U3;
NET "IN[0]" IOSTANDARD = LVCMOS18; NET "IN[0]" LOC = T3;
NET "Y[2]" IOSTANDARD = LVCMOS18; NET "Y[2]" LOC = R1;
NET "Y[1]" IOSTANDARD = LVCMOS18; NET "Y[1]" LOC = P2;
NET "Y[0]" IOSTANDARD = LVCMOS18; NET "Y[0]" LOC = P1;
NET "Done" IOSTANDARD = LVCMOS18; NET "Done" LOC = M1;
NET "EN" IOSTANDARD = LVCMOS18; NET "EN" LOC = R4; 
NET "EN" PULLDOWN;NET "IN[7]" PULLDOWN;NET "IN[6]" PULLDOWN;
NET "IN[5]" PULLDOWN;NET "IN[4]" PULLDOWN;NET "IN[3]" PULLDOWN;
NET "IN[2]" PULLDOWN;NET "IN[1]" PULLDOWN;NET "IN[0]" PULLDOWN;
```

## 4位的数值比较器

```verilog
module compare_n(x,y,xgy,xsy,xey);
input[4-1:0] x,y;                    //实现n位 则需要将4改为n
output xgy,xsy,xey;
reg xgy,xsy,xey;    
parameter width=4;                    //实现n位将width的值设置为n；

always@ (x or y)                       //每当x，y变化时
    begin
            if (x==y)
                xey = 1;            //设置x=y的信号为1
            else 
                xey = 0;
                if(x>y)
                    xgy = 1;         //设置x>y的信号为1
                else
                    xgy = 0;     
                if(x<y)
                    xsy = 1;         //设置x<y的信号为1
                else
                    xsy = 0;
            end
            
        endmodule
```

```verilog

```

## 基本RS触发器

```verilog
module RSchufa(S,R,Q,P);
		input S,R;
		output reg P,Q;
		always @(*)
		if((~S)&R)begin Q=0;P=1;end
		else if(S&(~R))begin Q=1;P=0;end
		else if(S&R) begin P=1;Q=1;end
endmodule
```

```verilog
// 测试代码

	initial begin		
	
		R=1;S=0;
		#100;
		R=0;S=1;
		#100;
		R=0;S=0;
		#100;
		R=1;S=1;
		#100;
		R=1;S=0;
		#100;
		R=0;S=0;
		#100;
		R=0;S=1;
		#100;

	end  
endmodule
```

```verilog
NET "R" LOC = T3;
NET "S" LOC = U3;
NET "P" LOC = P2;
NET "Q" LOC = R1;

NET "P" IOSTANDARD = LVCMOS18;
NET "Q" IOSTANDARD = LVCMOS18;
NET "R" IOSTANDARD = LVCMOS18;
NET "S" IOSTANDARD = LVCMOS18;

NET "R" PULLDOWN;
NET "S" PULLDOWN;
```



## JK触发器

```verilog
module jk(J,K,CLK,Q,Qn);
	  input J,K,CLK;
	  output Q,Qn;
	  
	  reg Q=0;
	  assign Qn=~Q;
	  
	  always@(negedge CLK)
	    case({J,K})
		    2'b00:Q<=Q;
			2'b01:Q<=0;
			2'b10:Q<=1;
			2'b11:Q<=~Q;
			
	  endcase
endmodule
```

```verilog
// 引脚
NET "CLK" LOC = R4;
NET "J" LOC = T3;
NET "K" LOC = U3;
NET "Q" LOC = R1;
NET "Qn" LOC = P2;

NET "CLK" IOSTANDARD = LVCMOS18;
NET "J" IOSTANDARD = LVCMOS18;
NET "K" IOSTANDARD = LVCMOS18;
NET "Q" IOSTANDARD = LVCMOS18;
NET "Qn" IOSTANDARD = LVCMOS18;

NET "CLK" PULLDOWN;
NET "J" PULLDOWN;
NET "K" PULLDOWN;
```

