# 4-bit-ALU-using-VHDL

Project Overview:
The ALU performs fundamental arithmetic and logic operations such as addition, subtraction, AND, OR, XOR, and NOT on 4-bit inputs.

Steps Involved:

Design and Coding: Developed the VHDL code to define the ALU operations.
Simulation: Verified the functionality using Vivado’s simulation tools.
Synthesis and Implementation: Implemented the design on an FPGA using Xilinx Vivado.
Verification on FPGA: Tested the ALU on an FPGA development board.
Key Learnings:

Gained hands-on experience with VHDL and FPGA design flow.
Enhanced understanding of digital logic circuits and hardware description languages.

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_ARITH.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity ALU is
    Port ( A : in  STD_LOGIC_VECTOR (3 downto 0);
           B : in  STD_LOGIC_VECTOR (3 downto 0);
           ALU_Sel : in  STD_LOGIC_VECTOR (2 downto 0);
           ALU_Out : out  STD_LOGIC_VECTOR (3 downto 0);
           CarryOut : out  STD_LOGIC );
end ALU;

architecture Behavioral of ALU is
begin
    process(A, B, ALU_Sel)
    begin
        case ALU_Sel is
            when "000" => -- Addition
                ALU_Out <= A + B;
                CarryOut <= '1' when (A + B) > "1111" else '0';
            when "001" => -- Subtraction
                ALU_Out <= A - B;
                CarryOut <= '0';
            when "010" => -- AND
                ALU_Out <= A and B;
                CarryOut <= '0';
            when "011" => -- OR
                ALU_Out <= A or B;
                CarryOut <= '0';
            when "100" => -- XOR
                ALU_Out <= A xor B;
                CarryOut <= '0';
            when "101" => -- NOT A
                ALU_Out <= not A;
                CarryOut <= '0';
            when others =>
                ALU_Out <= (others => '0');
                CarryOut <= '0';
        end case;
    end process;
end Behavioral;

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity ALU_tb is
end ALU_tb;

architecture Behavioral of ALU_tb is

    -- Component Declaration for the Unit Under Test (UUT)
    component ALU
    Port ( A : in  STD_LOGIC_VECTOR (3 downto 0);
           B : in  STD_LOGIC_VECTOR (3 downto 0);
           ALU_Sel : in  STD_LOGIC_VECTOR (2 downto 0);
           ALU_Out : out  STD_LOGIC_VECTOR (3 downto 0);
           CarryOut : out  STD_LOGIC );
    end component;

    -- Inputs
    signal A : STD_LOGIC_VECTOR(3 downto 0) := (others => '0');
    signal B : STD_LOGIC_VECTOR(3 downto 0) := (others => '0');
    signal ALU_Sel : STD_LOGIC_VECTOR(2 downto 0) := (others => '0');

    -- Outputs
    signal ALU_Out : STD_LOGIC_VECTOR(3 downto 0);
    signal CarryOut : STD_LOGIC;

begin
    -- Instantiate the Unit Under Test (UUT)
    uut: ALU Port map (
        A => A,
        B => B,
        ALU_Sel => ALU_Sel,
        ALU_Out => ALU_Out,
        CarryOut => CarryOut
    );

    -- Stimulus process
    stim_proc: process
    begin
        -- Test Addition
        A <= "0010"; B <= "0011"; ALU_Sel <= "000";
        wait for 10 ns;

        -- Test Subtraction
        A <= "0100"; B <= "0010"; ALU_Sel <= "001";
        wait for 10 ns;

        -- Test AND
        A <= "1100"; B <= "1010"; ALU_Sel <= "010";
        wait for 10 ns;

        -- Test OR
        A <= "1100"; B <= "1010"; ALU_Sel <= "011";
        wait for 10 ns;

        -- Test XOR
        A <= "1100"; B <= "1010"; ALU_Sel <= "100";
        wait for 10 ns;

        -- Test NOT
        A <= "1100"; ALU_Sel <= "101";
        wait for 10 ns;

        wait;
    end process;
end Behavioral;

