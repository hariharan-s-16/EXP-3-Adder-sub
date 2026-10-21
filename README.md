# EXP-3-Adder-sub
## Design and Verification of Half Adder and Half Subtractor
### Aim
To design and verify basic one-bit arithmetic circuits using Verilog HDL.
### Theory / Synopsis
A Half Adder adds two single-bit binary numbers, A and B, and produces a Sum and a Carry output. A Half Subtractor subtracts one single-bit binary number B from another A, producing a Difference and a Borrow output. Both circuits are purely combinational and form the basic building blocks of larger arithmetic units such as Full Adders and Full Subtractors.
Boolean Functions
Half Adder:
•	Sum = A ⊕ B
•	Carry = A . B
Half Subtractor:
•	Difference = A ⊕ B
•	Borrow = A' . B

### Files to be Created
File name	Purpose
half_add_sub.v	Arithmetic-circuit RTL
half_add_sub_tb.v	Stimulus and VCD generation

### Verilog Program
```
module half_adder (
    input  wire A,
    input  wire B,
    output wire Sum,
    output wire Carry
);
    assign Sum   = A ^ B;   // XOR
    assign Carry = A & B;   // AND
endmodule
 
module half_subtractor (
    input  wire A,
    input  wire B,
    output wire Diff,
    output wire Borrow
);
    assign Diff   = A ^ B;      // XOR
    assign Borrow = (~A) & B;  // A' . B
endmodule
```
### Testbench Program
```
module tb3;
    reg A, B;
    wire Sum, Carry;
    wire Diff, Borrow;
 
    // Instantiate the Half Adder
    half_adder ha_uut (
        .A(A),
        .B(B),
        .Sum(Sum),
        .Carry(Carry)
    );
 
    // Instantiate the Half Subtractor
    half_subtractor hs_uut (
        .A(A),
        .B(B),
        .Diff(Diff),
        .Borrow(Borrow)
    );
 
    initial begin
        // ---- VCD dump setup ----
        $dumpfile("half_add_sub.vcd");   // name of the VCD file to be generated
        $dumpvars(0, tb3);                // dump all signals in this testbench hierarchy
 
        // ---- Apply all 4 input combinations ----
        $monitor("Time=%0t A=%b B=%b | Sum=%b Carry=%b | Diff=%b Borrow=%b",
                   $time, A, B, Sum, Carry, Diff, Borrow);
        A = 0; B = 0; #10;
        A = 0; B = 1; #10;
        A = 1; B = 0; #10;
        A = 1; B = 1; #10;
 
        #10 $finish;
    end
endmodule
```

### Truth Table


<img width="645" height="87" alt="TT3" src="https://github.com/user-attachments/assets/37e33bb3-d647-45d5-b610-c255d7e41b8b" />

### Simulation Procedure

STEP 1 – Open Terminal
Open a terminal in the experiment folder.

STEP 2 – Load Synopsys Environment
source /synopsys/start.sh

STEP 3 – Compile Using VCS
vcs half_add_sub.v half_add_sub_tb.v -full64
If compilation is successful, VCS generates the simulation executable:
simv

STEP 4 – Run Simulation
./simv
The terminal displays the input combinations and corresponding outputs Sum, Carry, Diff, and Borrow.
A VCD waveform file is also generated:
half_add_sub.vcd

STEP 5 – Open DVE
dve -full64
Other option
dve -full64 &
A DVE environment will open.

DVE Waveform Verification
In DVE:
•	Open the testbench hierarchy.
•	Locate the signals:
o	A
o	B
o	Sum
o	Carry
o	Diff
o	Borrow
•	Add the signals to the waveform window.
•	Run/inspect the waveform.

•	Verify that Sum = 1 whenever A and B are different (A⊕B), and Carry = 1 only when A=1 AND B=1.

•	Verify that Diff = 1 whenever A and B are different (same XOR pattern as Sum), and Borrow = 1 only when A=0 AND B=1.

•	Confirm that Carry and Borrow never both go high for the same input pair, since they represent different (AND vs A'B) conditions.

The waveform should agree with the truth table.

### Expected Result

<img width="1920" height="1080" alt="Screenshot (19)" src="https://github.com/user-attachments/assets/5097bbf7-e47b-4c90-ac42-985a7931e764" />

### Output
<img width="1920" height="1080" alt="Screenshot (20)" src="https://github.com/user-attachments/assets/eea67514-0f3d-4df9-9826-6f0aec502c6e" />


### Viva-Voce Questions
Q: What is the primary difference between a Half Adder and a Full Adder?

A: A Half Adder adds two single-bit inputs (
𝐴
A and 
𝐵
B) and produces two outputs: Sum and Carry. A Full Adder can add three inputs (including a Carry-in from a previous stage) and produce a Sum and a Carry-out.

Q: Why are Sum and Diff identical in terms of their Boolean logic for half arithmetic circuits?

A: Both the Sum of a half adder (
𝐴
⊕
𝐵
A⊕B) and the Difference of a half subtractor (
𝐴
⊕
𝐵
A⊕B) rely on the XOR operation. Mechanically, the bitwise inequality of two bits results in a 1 for both addition and subtraction.

Q: What is the purpose of $dumpfile and $dumpvars system tasks in your testbench?

A: $dumpfile("half_add_sub.vcd") specifies the name of the Value Change Dump file where waveform data is stored, and $dumpvars(0, tb3) instructs the simulator to record all hierarchical variable changes inside the tb3 module for waveform analysis in tools like Synopsys DVE.

Q: How does a Half Subtractor handle the Borrow output when 
A=0 and B=1?
A: When 𝐴=0 𝐵=1,subtracting 
1
1 from 
0
0 requires a borrow from the next higher-order bit. The Boolean expression for Borrow is 
𝐴
ˉ
𝐵
A
ˉ
 B, which evaluates to 1 under this specific condition.

Q: What is the purpose of the $monitor system task in the initial block?

A: $monitor automatically prints formatted text to the console whenever any of the variables listed in its sensitivity list ($time, A, B, Sum, Carry, Diff, Borrow) change state, allowing for easy textual verification during simulation.

### Result

Thus, the Half Adder and Half Subtractor were designed, implemented using Verilog HDL, successfully simulated using Synopsys VCS, and verified using Synopsys DVE.





