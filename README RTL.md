```verilog
module jk_ff_async_rst_n (input wire j,k,clk,rst_n,
                          output reg q, 
                          output wire q_bar);
  
  always@(posedge clk or negedge rst_n) begin
    if(rst_n == 0)
      q <= 0;
    else case({j,k})
     2'b00: q <= q;
     2'b01: q <= 1'b0;
     2'b10: q <= 1'b1;
     default: q <= ~q;
    endcase
  end
 assign q_bar = ~q; 
endmodule

module ctrl_up_down( input wire q, q_bar,
                     input bit up,
                    output wire nclk);
  
  assign nclk = up ? q_bar:q;
endmodule
      
  

module up_down_async_counter_4bit #(parameter N=4) ( input wire j,k,clk,rst_n,up, 
                                                    output wire [N-1:0] q, q_bar);
  
  wire [N-1:0]nclk;
  genvar i;
  
  jk_ff_async_rst_n jk_ff0(j ,k ,clk ,rst_n ,q[0] ,q_bar[0]);
  generate
  for(i=1; i<N; i=i+1) begin
    ctrl_up_down c_u_d (q[i-1] ,q_bar[i-1] ,up ,nclk[i-1]);
    jk_ff_async_rst_n jk_ff(j ,k ,nclk[i-1] ,rst_n ,q[i] ,q_bar[i]);
  end
  endgenerate
endmodule
    
