**Design.sv**

/*creating a finite state machine with clock, reset, one input and one output*/
module streetlightFSM(input logic clk,
                      input logic reset,
                      input logic X,
                      output logic Y);
  
  typedef enum logic [1:0] {S0, S1, S2, S3} State;
  /*four states with only 2 bits needed*/
  
  State currentState, nextState;
  /*declare current & next state */
  
  /*state will only change during the rising edge of clock*/
  /*reset state to S0 if reset is used*/
  always_ff @(posedge clk)
    if(reset) currentState <= S0;
  	else      currentState <= nextState;
  
  /*combinational logic only*/
  always_comb
    
    /*case statement used to change states*/ 
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

/*Creating a testbench to simulate operation of FSM*/
module testbenchFSM;
  logic clk;
  logic reset;
  logic in;
  logic out;
  /*connecting wires with inputs from design file*/
  testbenchFSM dut(
    .clk(clk),
    .reset(reset),
    .X(in),
    .Y(out)
  );
  /*to run the EPWave after running*/
  initial begin
    $dumpfile("dump.vcd"); 
    $dumpvars(1);
  end
  /*test inputs with a 10 nanosecond delay between*/
  initial begin
      reset <= 1; #10;
      reset <= 0; in <= 0; #10;
      in <= 1; #10;
      in <= 1; #10;
      in <= 0; #10;
      in <= 1; #10;
      in <= 1; #10;
    end
  /*clock will rise and fall with a period of 10 ns total*/
  always
    begin
      clk <=1; #5;
      clk <=0; #5;
    end
  
endmodule
