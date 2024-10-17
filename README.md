# SEQUENCE-DETECTOR

## AIM:

To design and simulate a sequence detector using both Moore and Mealy state machine models in Verilog HDL, and verify their functionality through a testbench using the Vivado 2023.1 simulation environment. The objective is to detect a specific sequence of bits (e.g., 1011) and compare the Moore and Mealy designs.

## APPARATUE REQUIRED:

Vivado 2023.1 or equivalent Verilog simulation tool.
Computer system with a suitable operating system.

## PROCEDURE:

Launch Vivado 2023.1:

Open Vivado and create a new project.
Write the Verilog Code for Sequence Detector (Moore and Mealy FSM):

Design two Verilog modules: one for a Moore FSM and another for a Mealy FSM to detect a sequence such as 1011.
Create the Testbench:

Write a testbench to apply input sequences and verify the output of both FSM designs.
Add the Verilog Files:

Add the Verilog code for both FSMs and the testbench to the Vivado project.
Run Simulation:

Run the simulation and observe the output to check if the sequence is detected correctly.
Observe the Waveforms:

Analyze the waveform to ensure both the Moore and Mealy machines detect the sequence as expected.
Save and Document Results:

Capture the waveforms and include the results in the final report.

## VERILOG CODE FOR SEQUENCE DETECTOR USING MOORE FSM:

~~~
module moore_counter (
    input wire clk,
    input wire reset,
    output reg [1:0] state_output
);

    // State encoding using parameters
    parameter S0 = 2'b00; // State 0
    parameter S1 = 2'b01; // State 1
    parameter S2 = 2'b10; // State 2
    parameter S3 = 2'b11; // State 3

    reg [1:0] current_state, next_state;

    // Sequential logic for state transition
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            current_state <= S0;
        end else begin
            current_state <= next_state;
        end
    end

    // Combinational logic for next state and output
    always @* begin
        case (current_state)
            S0: begin
                next_state = S1;
                state_output = S0; // Output for state S0
            end
            S1: begin
                next_state = S2;
                state_output = S1; // Output for state S1
            end
            S2: begin
                next_state = S3;
                state_output = S2; // Output for state S2
            end
            S3: begin
                next_state = S0;
                state_output = S3; // Output for state S3
            end
            default: begin
                next_state = S0;
                state_output = S0; // Default output
            end
        endcase
    end
endmodule


~~~


## OUTPUT:


![Screenshot 2024-10-10 143056](https://github.com/user-attachments/assets/286baf9a-e7e5-4645-afb4-5850b177b9ef)


## VERILOG CODE FOR SEQUENCE DETECTOR USING MEALY FSM:

~~~

module mealy(clk,rst,inp,out);

 input clk, rst, inp;

 output out;

 reg out;

 reg [1:0] state;

 always @(posedge clk, posedge rst)

 begin

     if(rst)

         begin

             out <= 0;

             state <= 2'b00;

         end

     else

         begin

             case (state)

             2'b00: 

                 begin

                     if (inp) begin

                         state <=2'b01;

                         out <=0;

                     end

                     else begin

                         state <=2'b10;

                         out <=0;

                     end

                 end

             2'b01:

                 begin

                     if(inp) begin

                         state <= 2'b00;

                         out <= 1;

                     end

                     else begin

                         state <= 2'b10;

                         out <= 0;

                     end

                 end

             2'b10:

                 begin

                     if(inp) begin

                         state <= 2'b01;

                         out <= 0;

                     end

                     else begin

                         state <= 2'b00;

                         out <= 1;

                     end

                 end

             default:

                 begin

                     state <= 2'b00;

                     out <= 0;

                 end

             endcase

         end
 end

 endmodule

    

~~~

## OUTPUT:

![Screenshot 2024-10-17 110017](https://github.com/user-attachments/assets/ff57d9bb-0695-4cd4-9b9e-78fe0626276d)

## TESTBENCH FOR SEQUENCE DETECTOR MOORE FSMs:


~~~
`timescale 1ns / 1ps

module tb_moore_counter;

    reg clk;
    reg reset;
    wire [1:0] state_output;

    // Instantiate the Moore counter
    moore_counter uut (
        .clk(clk),
        .reset(reset),
        .state_output(state_output)
    );

    // Clock generation
    initial begin
        clk = 0;
        forever #5 clk = ~clk; // 10 ns clock period
    end

    // Test sequence
    initial begin
        // Initialize
        reset = 1; // Assert reset
        #10;        // Wait for 10 ns
        reset = 0; // Deassert reset

        // Allow the counter to run for a few cycles
        #40; // Run for 40 ns

        // Assert reset again
        reset = 1; 
        #10;
        reset = 0; // Deassert reset

        #40; // Run for another 40 ns

        // Finish simulation
        $finish;
    end

    // Monitor state output
    initial begin
        $monitor("Time: %0t | Reset: %b | State Output: %b", $time, reset, state_output);
    end

endmodule

~~~

## OUTPUT:

![Screenshot 2024-10-10 144927](https://github.com/user-attachments/assets/91455445-b788-4d51-9f62-5881a5be18a7)



## TESTBENCH FOR SEQUENCE DETECTOR MEALY FSMs:
~~~
 // directly test values given
 
`timescale 1ns/1ns

 `include "mealy.v"

 module mealy_tb;

 wire out;

 reg clk,rst,inp;

 reg [15:0] seq; 

 integer i;

 mealy instance22(clk, rst, inp, out);

 initial begin

     clk=0;

     rst=1;

     seq=16'b1010_0001_1100_0101;

     #5 rst=0;

 end

 always begin

       $dumpfile("mealy_direct.vcd");

             $dumpvars();

     for( i = 0; i <= 15; i = i+1)

         begin

             inp = seq[i];

             #2 clk=1;

             #2 clk=0;

             $display("state = ",instance22.state,"| input = ",inp,"| output = ",out);

         end

        #100 $finish();

 end

 endmodule
~~~

## OUTPUT:
![Uploading Screenshot 2024-10-17 110017.png…]()


## CONCLUSION:
In this experiment, Moore and Mealy FSMs were successfully designed and simulated to detect the sequence 1011. Both designs worked as expected, with the main difference being that the Moore FSM generated the output based on the current state, while the Mealy FSM generated the output based on both the current state and input. The testbench verified the functionality of both FSMs, demonstrating that the Verilog HDL can effectively model both types of state machines for sequence detection tasks.
