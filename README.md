SPC-700 SMP (CPU) Tests
=======================
These are some builds of a suite of SMP tests. They can be loaded into RAM and executed. I never had the time/energy to make a polished release, so meh. I haven't worked on them in many years so this is what I've put together based on my old project code:

1. Clear 64K RAM.
2. Load file's 0x2000 bytes at address 0x400.
3. Set program counter to 0x400.
4. Be sure DSP is properly muted so it isn't modifying echo region in RAM.
5. Start emulating.

During emulation, the test will place some values in memory to indicate it's running and its progress.

First, the addresses 0x8001, 0x8002, and 0x8003 must contain 0xDE 0xB0 0x61. If they don't, none of the others have any meaning.

Then, 0x8000 contains the status; 0x80 means the test is busy, any other value and the test is done is the result code. 0 = success, non-zero = error code.

During test execution any text output will be appended to a zero-terminated string at 0x8004. It's simplest to just wait and print this once the test has completed or it's taken more than 20 seconds of emulated time.


output.txt contains a log from my old test program running the tests. Some of them rely on accurate SMP timer behavior and test the timing of individual reads/writes in instructions.

CPU Instructions_Edge arith: prints opcode and checksum. Compare checksums with output.

CPU Instructions_Full BRK, CPU Instructions_Full CMP, CPU Instructions_Full DAA DAS: check indicated instructions for all(?) cases.

CPU_addw and subw: first line is ADDW adding the left two columns, with PSW the third before the add; next word is result and final byte is PSW after.
Second line is SUBW with same conditions.
Third line is ADDW with new inputs, etc.

CPU_psw is 8 independent bits: verifies that all 8 bits of PSW exist and can be set independently.

CPU_smp reg read-write behavior: Tests that only $F2, $F3, $F8, and $F9 read back the last data written.

CPU_tset tclr: TSET and TCLR set N and Z based on A - operand (like CMP)

CPU_verify IPL ROM: verifies IPL ROM

CPU_wrap-around mem: verifies memory wrapping around from 0xFFFF to 0.

CPU_wrap-around pc: verifies that wrapping execution around works

CPU_wrap-around sp: verifies SP wrapping around

CPUIO execute: verifies that execution can run 

-- 
Shay Green <gblargg@gmail.com>
