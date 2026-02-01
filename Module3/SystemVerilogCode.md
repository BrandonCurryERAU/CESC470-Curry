**Design.sv**

module streetlightFSM(input logic clk,
                      input logic reset,
                      input logic X,
                      output logic Y);
  
  typedef enum logic [1:0] {S0, S1, S2, S3} State;
  
  State currentState, nextState;
  
  always_ff @(posedge clk)
    if(reset) currentState <= S0;
  	else      currentState <= nextState;
  
  always_comb
    
    case(currentState)
      S0: if(X) nextState = S1;
      		else nextState = S0;
      S1: if(X) nextState = S2;
      		else nextState = S2;
      S2: if(X) nextState = S3;
      		else nextState = S2;
      S3: if(X) nextState = S0;
      		else nextState = S0;
      
      default:   nextState = S0;
      
    endcase
      
endmodule

**Testbench code**

module testbenchFSM;
  logic clk;
  logic reset;
  logic in;
  logic out;

  testbenchFSM dut(
    .clk(clk),
    .reset(reset),
    .X(in),
    .Y(out)
  );

  initial begin
    $dumpfile("dump.vcd"); 
    $dumpvars(1);
  end

  initial begin
      reset <= 1; #10;
      reset <= 0; in <= 0; #10;
      in <= 1; #10;
      in <= 1; #10;
      in <= 0; #10;
      in <= 1; #10;
      in <= 1; #10;
    end

  always
    begin
      clk <=1; #5;
      clk <=0; #5;
    end
  
endmodule
