# RAM
    module dual_port_ram 
   
   
     #( parameter ADDR_WIDTH = 4,
     parameter DATA_WIDTH = 16,
     parameter DEPTH = 16
     )
 
    (input clk,
    input [ADDR_WIDTH-1:0] 	addr1,
    input [ADDR_WIDTH-1:0] 	addr2,
    input [DATA_WIDTH-1:0] 	data1,
    input [DATA_WIDTH-1:0] 	data2,
    input cs1,
    input cs2,
    input we1,
    input we2,
    input oe1,
    input oe2,
    output [DATA_WIDTH-1:0] 	q1,
    output [DATA_WIDTH-1:0] 	q2
    );
  
    reg [DATA_WIDTH-1:0] 	mem [DEPTH-1:0];
  
    always @ (posedge clk) begin
     if (cs1 & we1)
      mem[addr1] <= data1; 
      
    if (cs2 & we2)
      mem[addr2] <= data2; 
 
    end
  
    always @ (negedge clk) begin
     if (cs1 & oe1)
      q1 <= mem[addr1];     // read from address 1
      
    if (cs2 & oe2)
      q2 <= mem[addr2];     // read from address 2
    end
  
    assign q1 = cs1 & oe1 ? q1 : 'hz;  //  address 1
    assign q2 = cs2 & oe2 ? q2 : 'hz;  //  address 2
    endmodule
