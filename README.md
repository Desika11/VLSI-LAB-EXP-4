VLSI-LAB-EXP-4
SIMULATION AND IMPLEMENTATION OF SEQUENTIAL LOGIC CIRCUITS

AIM: 
To simulate and synthesis SR, JK, T, D - FLIPFLOP, COUNTER DESIGN using vivado 2023.1.

APPARATUS REQUIRED:
vivado 2023.1.

**LOGIC DIAGRAM**

SR FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/77fb7f38-5649-4778-a987-8468df9ea3c3)


JK FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/1510e030-4ddc-42b1-88ce-d00f6f0dc7e6)

T FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/7a020379-efb1-4104-85ee-439d660baa08)


D FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/dda843c5-f0a0-4b51-93a2-eaa4b7fa8aa0)


COUNTER

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/a1fc5f68-aafb-49a1-93d2-779529f525fa)


  
PROCEDURE:
Open Vivado: Launch Xilinx Vivado software on your computer.

Create a New Project: Click on "Create Project" from the welcome page or navigate through "File" > "Project" > "New".

Project Settings: Follow the prompts to set up your project. Specify the project name, location, and select RTL project type.

Add Design Files: Add your Verilog design files to the project. You can do this by right-clicking on "Design Sources" in the Sources window, then selecting "Add Sources". Choose your Verilog files from the file browser.

Specify Simulation Settings: Go to "Simulation" > "Simulation Settings". Choose your simulation language (Verilog in this case) and simulation tool (Vivado Simulator).

Run Simulation: Go to "Flow" > "Run Simulation" > "Run Behavioral Simulation". This will launch the Vivado Simulator and compile your design for simulation.

Set Simulation Time: In the Vivado Simulator window, set the simulation time if it's not set automatically. This determines how long the simulation will run.

Run Simulation: Start the simulation by clicking on the "Run" button in the simulation window.

View Results: After the simulation completes, you can view waveforms, debug signals, and analyze the behavior of your design.

VERILOG CODE
 Dflipflop
~~~
module dff(d,clk,rst,q);
input d,clk,rst;
output reg q;
always @(posedge clk)
begin
if (rst==1)
q=1'b0;
else
q=d;
end
endmodule
~~~

OUTPUT WAVEFORM
 ![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/85b4f953-c78c-4631-848b-ec83ef292a6d)
![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/ec57022a-89fb-4239-ab86-e7c99594869a)

JKflipflop
~~~
module JK_flipflop (q, q_bar, j,k, clk, reset);  
  input j,k,clk, reset;
  output reg q;
  output q_bar;
  // always@(posedge clk or negedge reset) // for asynchronous reset
  always@(posedge clk) begin // for synchronous reset
    if(!reset)
           q <= 0;
    else 
  begin
      case({j,k})
        2'b00: q <= q;    // No change
        2'b01: q <= 1'b0; // reset
        2'b10: q <= 1'b1; // set
        2'b11: q <= ~q; // Toggle
      endcase
    end
  end
  assign q_bar = ~q;
endmodule
~~~

OUTPUT:
![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/2641d9b3-6da7-48c4-b9b8-73578fa3a59e)
![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/edaab32c-28c3-4622-b4e8-31bca5b55c82)

Mod10Counter
~~~
module counter(
input clk,rst,enable,
output reg [3:0]counter_output
);
always@ (posedge clk)
begin 
if( rst | counter_output==4'b1001)
counter_output <= 4'b0000;
else if(enable)
counter_output <= counter_output + 1;
else
counter_output <= 0;
end
endmodule
~~~

OUTPUT:
![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/15fade93-1aa4-48f0-afee-c52cf513a983)
![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/cd46b0cd-7b14-4866-bd33-6757ae4e7370)

Ripplecarrycounter
~~~
module D_FF(q, d, clk, reset);
output q;
input d, clk, reset;
reg q;
always @(posedge reset or negedge clk)
if (reset)
q = 1'b0;
else
q = d;
endmodule
module T_FF(q, clk, reset);
output q;
input clk, reset;
wire d;
D_FF dff0(q, d, clk, reset);
not n1(d, q); 
endmodule
module ripple_carry_counter(q, clk, reset);
output [3:0] q;
input clk, reset;
T_FF tffo(q[0], clk, reset);
T_FF tff1(q[1], q[0], reset);
T_FF tff2(q[2], q[1], reset);
T_FF tff3(q[3], q[2], reset);
endmodule
~~~

OUTPUT:
![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/370174ef-9789-4efa-868c-ab3599d852cc)
![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/180f5c0e-d5b4-4cb3-9d94-efab3719c189)

SRflipflop
~~~
module SR_flipflop (q, q_bar, s,r, clk, reset);
  input s,r,clk, reset;
  output reg q;
  output q_bar;
  always@(posedge clk) begin 
    if(!reset)�        q <= 0;
    else 
  begin
      case({s,r})
        2'b00: q <= q;    
        2'b01: q <= 1'b0; 
        2'b10: q <= 1'b1; 
        2'b11: q <= 1'bx; 
      endcase
    end
  end
  assign q_bar = ~q;
endmodule
~~~

OUTPUT:
![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/157f41c2-85cb-4fcb-8c6a-c19244d7727d)
![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/0492f1a7-77f0-4853-8cdf-1645055177ab)


T flipflop
~~~
module tff (t,clk, rstn,q);  
 input t,clk, rstn;
 output reg q;
  always @ (posedge clk) begin  
    if (!rstn)  
      q <= 0;  
    else  
        if (t)  
            q <= ~q;  
        else  
            q <= q;  
  end  
endmodule
~~~

OUTPUT:
![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/880a2c33-2c5c-4736-9516-5cc2459a8d9d)
![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/51835935-3719-4e48-afea-e9e2ca6a254f)


Updowncounter
~~~
module updown_counter(clk,rst,updown,out);
input clk,rst,updown;
output reg [3:0]out;
always@(posedge clk)
begin
if (rst==1)
out=4'b0000;
else if(updown==1)
out=out+1;
else
out=out-1;
end
endmodule
~~~

OUTPUT:
![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/b91b1389-e893-414a-979d-d48a1476c084)
![image](https://github.com/Desika11/VLSI-LAB-EXP-4/assets/165646570/ad0cc54c-0a32-4726-beb7-e2236b3ac10b)

RESULT:
Simulate And Synthesis SR, JK, T, D - FLIPFLOP, COUNTER DESIGN is Successfully Verified using Vivado Software.


