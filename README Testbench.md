```verilog
`timescale 1ns/1ps
module tb_up_down_async_counter_4bit;
  
  parameter N=4;
  reg j,k,clk,rst_n,up;
  wire [N-1:0]q, q_bar;
  
  up_down_async_counter_4bit #(N) DUT (j,k,clk,rst_n,up,q,q_bar);
  
  always #2 clk= ~clk;
  
  initial begin
    $monitor("time=%0t, rst_n=%b ,clk=%b, j=%b ,k=%b ,up=%b | q=%b | q_bar=%b",$time,rst_n,clk,j,k,up,q,q_bar);
    
  rst_n = 0;
  clk = 0;
  up = 1;
  j = 1 ;k = 1;
  
  #1 rst_n = 1;
 
  
  #20 rst_n = 0;
  
  #30  rst_n = 1;
  up = 0;
    
  #50;
  
  $finish;
  end
  
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars;
  end
endmodule
  
  
