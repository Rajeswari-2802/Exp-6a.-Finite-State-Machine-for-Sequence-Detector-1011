# Exp-6a.-Finite-State-Machine-for-Sequence-Detector-1011
# Aim 
To design and simulate a Finite-State-Machine-for-Sequence-Detector-1011 using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment. 

# Apparatus Required 

Vivado 2023.1 

# Procedure

Launch Vivado 2023.1 Open Vivado and create a new project.
Design the Verilog Code Write the Verilog code for the RAM,ROM,FIFO
Create the Testbench Write a testbench to simulate the memory behavior. The testbench should apply various and monitor the corresponding output.
Create the Verilog Files Create both the design module and the testbench in the Vivado project.
Run Simulation Run the behavioral simulation to verify the output.
Observe the Waveforms Analyze the output waveforms in the simulation window, and verify that the correct read and write operation.
Save and Document Results Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.
# Mealy 1011
**Verilog code**
````
module segm(
input clk,rst,xin,
output reg zout);
parameter [2:0] 
s1 = 3'b000,
s2 = 3'b001,
s3 = 3'b010,
s4 = 3'b011;
reg [2:0] ps,ns;
always@(posedge clk)
begin
if(rst)
ps <= s1;
else
ps <= ns;
end      
always@(xin or ps)
begin 
case(ps)
s1 : if(xin) 
begin
ns = s2;
zout = 0;
end             
else  
begin
ns = s1;
zout = 0;
end 
s2 : if(xin) 
begin
ns = s2;
zout = 0;
end             
else  
begin
ns = s3;
zout = 0;
end             
s3 : if(xin) 
begin
ns = s4;
zout = 0;
end             
else  
begin
ns = s1;
zout = 0;
end              
s4 : if(xin) begin
ns = s1;
zout = 1;
end             
else
begin
ns = s3;
zout = 0;
end              
endcase
end
endmodule
````
**Test bench**
````
module segm_tb;
reg clk_t, rst_t, xin_t;
wire zout_t;
segm dut (
        .clk(clk_t),
        .rst(rst_t),
        .xin(xin_t),
        .zout(zout_t));
always #50 clk_t = ~clk_t;
initial begin
clk_t = 1'b0;
rst_t = 1'b1;
xin_t = 1'b0;
#100 rst_t = 1'b0;
#100 xin_t = 1'b1;
#100 xin_t = 1'b0;
#100 xin_t = 1'b1;
#100 xin_t = 1'b1;
#100 xin_t = 1'b1;
#100 xin_t = 1'b0;
#100 xin_t = 1'b1;
#100 xin_t = 1'b1;
#200 $finish;
end
initial begin
$monitor("Time=%0t | clk=%b | rst=%b | xin=%b | zout=%b",
$time, clk_t, rst_t, xin_t, zout_t);
end
endmodule
````

**output Waveform**

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cf975092-f6b2-4e23-934e-0454b9e2bacf" />


# Moore 1011
**CODE**
```
module seq (
input clk,
input rst,     
input xin,      
output reg z );
parameter [2:0]
S0 = 3'b000,   
S1 = 3'b001,   
S2 = 3'b010,   
S3 = 3'b011,   
S4 = 3'b100;   
reg [2:0] ps, ns;
always @(posedge clk or posedge rst)
begin
if (rst)
ps <= S0;
else
ps <= ns;
end
always @(ps, xin)
begin
ns = S0;
z  = 1'b0;
case (ps)
S0: begin
if (xin)
ns = S1; 
else
ns = S0;
end
S1: begin
if (xin)
ns = S1; 
else
ns = S2;
end
S2: begin
if (xin)
ns = S3; 
else
ns = S0; 
end
S3: begin
if (xin)
ns = S4; 
else
ns = S2; 
end
S4: begin
z = 1'b1;
if (xin)
ns = S1;  
else
ns = S2;  
end
endcase
end
endmodule
```
**Testbench**
```
module seq_tb;
reg clk;
reg rst;
reg xin;
wire z;
seq uut (
.clk(clk),
.rst(rst),
.xin(xin),
.z(z));
always #5 clk = ~clk;
initial begin
clk = 0;
rst = 1;
xin = 0;
#10 rst = 0;
#10 xin = 1;   
#10 xin = 0;   
#10 xin = 1;   
#10 xin = 1;  
#10 xin = 0;
#10 xin = 1;
#10 xin = 1;
#50 $stop;
end
initial begin
$monitor("Time=%0t | clk=%b | rst=%b | xin=%b | z=%b | state=%b",
$time, clk, rst, xin, z, uut.ps);
end
endmodule

```
**output Waveform**

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7b8091a1-5ed1-4c5a-9187-7ca0e40c3ad6" />

# Conclusion

The Mealy and Moore state machine for sequence 1011 was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the sequence operations and observing the output waveforms.
