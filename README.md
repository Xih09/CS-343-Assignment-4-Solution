Download link : https://programming.engineering/product/cs-343-assignment-4-solution/

CS 343 Assignment 4 Solution
This assignment introduces complex locks in µC++ and continues examining synchronization and mutual exclusion.

Use it to become familiar with these new facilities, and ensure you use these concepts in your assignment solution.

(Tasks may NOT have public members except for constructors and/or destructors.)

    Figure 1 is a C++ program comparing buffering using internal-data versus external-data format. Discard the program output by using shell redirect to /dev/null, e.g., a.out . . . > /dev/null

        Compare the three versions of the program with respect to performance by doing the following:

            Run the program with preprocessor variables –DARRAY, –DSTRING and –DSTRSTREAM.

            Time the executions using the time command:

    /usr/bin/time –f “%Uu %Ss %E” ./a.out > /dev/null # ignore program output

3.21u 0.02s 0:03.32

(Output from time differs depending on the shell, so use the system time command.) Compare the user time (3.21u) only, which is the CPU time consumed solely by the execution of user code (versus system and real time).

            Use the command-line arguments 1000000 20 and adjust the times amount (if necessary) to get pro-gram execution into the range 1 to 100 seconds. (Timing results below 1 second are inaccurate.) Use the same command-line values for all experiments, if possible; otherwise, increase/decrease the arguments as necessary and scale the difference in the answer.

            Run both the experiments again after recompiling the programs with compiler optimization turned on (i.e., compiler flag –O2).

            Include all 6 timing results to validate the experiments and the number of calls to malloc.

        State the performance and allocation difference (larger/smaller/by how much) between the three versions of the program.

        State the performance difference (larger/smaller/by how much) when compiler optimization is used.

    Figure 2, p. 3 shows a solution to the mutual-exclusion problem by Harris Hyman that appeared in a letter (i.e., a non-refereed article) to the Communications of the ACM. Unfortunately it does not work.

        Explain which rule(s) of the critical-section game is broken and the steps resulting in failure.

        Compile with flags –multi –nodebug and run multiple times. Add flag –O2 and run multiple times. Explain why the broken rule(s) can take a long time to cause a failure or no failure at all during a large test.

    (a) Consider the following situation involving a tour group of V tourists. The tourists arrive at the Louvre museum for a tour. However, a tour group can only be composed of G people at a time, otherwise the tourists cannot hear the guide. As well, there are 3 kinds of tours available at the Louvre: pictures, statues and gift shop. Therefore, each tour group must vote to select the kind of tour to take. Voting is a ranked ballot, where each tourist ranks the 3 tours with values 0, 1, 2, where 2 is the highest rank. Tallying the votes sums the ranks for each kind of tour and selects the highest ranking. If tie votes occur among rankings, prioritize the results by gift shop, pictures, and then statues, e.g.:

            	

            P
            	

            S G

            tourist1
            	

            0
            	

            1
            	

            2

            tourist2
            	

            2
            	

            1
            	

            0

            tally
            	

            2
            	

            2 2 all ties, select G

CS 343 – Assignment 4 2

CS 343 – Assignment 4 6

        	
        	
        	
        	
        	
        	
        	
        		
        			
        			
        			
        			
        			
        			
        			
        			
        			
        			

				
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					
					

CS 343 – Assignment 4
	

7

value of “S”, the buffer must be flushed generating line 4. V1’s new value of “V 0,2,1” is then inserted into its buffer slot. When V1 attempts to print “C”, which overwrites its current buffer value of “V 0,2,1”, the buffer must be flushed generating line 5, and no other values are printed on the line because the print is consecutive (i.e., no intervening call from another object). Then V1 inserts value “C” and V0 inserts value “V 2,0,1” into the buffer. Assume V0 attempts to print “C”, which overwrites its current buffer value of “V 2,0,1”, the buffer must be flushed generating line 6, and so on. Note, a group size of 1 means a voter never has to block/unblock.

For example, in the right-hand example of Figure 8, there are 6 voters, 3 voters in a group, and each voter votes twice. Voters V3 and V4 are delayed (e.g., they went to Tom’s for a coffee and donut). By looking at the F codes, V0, V1, V5 vote together (group 1), V0, V1 V2 vote together (group 2), and V2, V4, V5 vote together (group 3). Hence, V0, V1, V2, and V5 have voted twice and terminated. V3 needs to vote twice and V4 needs to vote again. However, there are now insufficient voters to form a group, so both V3 and V4 fail with X.

The executable program is named vote and has the following shell interface:

vote [ voters | ’d’ [ group | ’d’ [ votes | ’d’ [ seed | ’d’ [ processors | ’d’ ] ] ] ] ]

voters is the size of a tour (> 0), i.e., the number of voters (tasks) to be started. If d or no value for voters is specified, assume 6.

group is the size of a tour group (> 0). If d or no value for group is specified, assume 3.

votes is the number of tours (> 0) each voter takes of the museum. If d or no value for votes is specified, assume 1.

seed is the starting seed for the random-number generator (> 0). If d or no value for seed is specified, initialize the random number generator with an arbitrary seed value (e.g., getpid() or time), so each run of the program generates different output.

processors is the number of processors (> 0) for parallelism. If d or no value for processors is specified, assume 1.

Use the monitor MPRNG to safely generate random values (monitors will be discussed shortly). Note, because of the non-deterministic execution of concurrent programs, multiple runs with a common seed may not generate the same output. Nevertheless, shorts runs are often the same so the seed can be useful for testing. Check all command arguments for correct form (integers) and range; print an appropriate usage message and terminate the program if a value is missing or invalid.

Add the following declaration to the program main immediately after checking command-line arguments but before creating any tasks:

uProcessor p[processors – 1]; // number of kernel threads

to adjust the amount of parallelism for computation. The default value for processors is 1. Since the program starts with one kernel thread, only processors – 1 additional kernel threads are necessary.

    Recompile the program with preprocessor option –DNOOUTPUT to suppress output.

        Compare the performance among the 3 kinds of locks by eliding all output (not even calls to the printer) and doing the following:

    Time the executions using the time command:

    /usr/bin/time –f “%Uu %Ss %Er %Mkb” vote 100 10 10000 1003

3.21u 0.02s 0:05.67r 32496kb

Output from time differs depending on the shell, so use the system time command. Compare the user (3.21u) and real (0:05.67r) time among runs, which is the CPU time consumed solely by the execution of user code (versus system) and the total time from the start to the end of the program.

    If necessary, adjust the number of votes to get real time in range 1 to 100 seconds. (Timing results below 1 second are inaccurate.) Use the same number of votes for all experiments.

    Include all 3 timing results to validate your experiments.

    Repeat the experiment using 2 processors and include the 3 timing results to validate your exper-iments.

CS 343 – Assignment 4
	

8

    State the performance difference (larger/smaller/by how much) among the locks as the kernel threads increase.

    Very briefly speculate on the performance difference, i.e., why is it the same or different.

Use the following to elide output:

#ifdef NOOUTPUT

#define PRINT( stmt )

#else

#define PRINT( stmt ) stmt

#endif // NOOUTPUT

Submission Guidelines

Follow these guidelines carefully. Review the Assignment Guidelines and C++ Coding Guidelines before starting each assignment. Each text or test-document file, e.g., *.{txt,doc} file, must be ASCII text and not exceed 500 lines in length, using the command fold –w120 *.doc | wc –l. Programs should be divided into separate compilation units, i.e., *.{h,cc,C,cpp} files, where applicable. Use the submit command to electronically copy the following files to the course account.

    q1*.txt – contains the information required by question 1, p. 1.

    q2*.txt – contains the information required by question 2, p. 1.

    BargingCheckVote.h – barging checker (provided)

    MPRNG.h – random number generator (provided)

    q3tallyVotes.h, q3*.{h,cc,C,cpp} – code for question question 3a, p. 1. Program documentation must be present in your submitted code. No user, system or test documentation is to be submitted for this question.

    q3*.txt – contains the information required by question 3b.

    Modify the following Makefile to compile the programs for question 3a, p. 1 by inserting the object-file names matching your source-file names.

VIMPL:=MC

OUTPUT:=OUTPUT

BCHECK:=NOBARGINGCHECK

CXX = u++ # compiler

CXXFLAGS = –g –multi –O2 –Wall –Wextra –MMD –D“${VIMPL}” –D“${OUTPUT}” \ –D“${BCHECK}” # compiler flags

MAKEFILE_NAME = ${firstword ${MAKEFILE_LIST}} # makefile name

OBJECTS = q3tallyVotes${VIMPL}.o # list of object files for question 3 prefixed with “q3” EXEC = vote

DEPENDS = ${OBJECTS:.o=.d} # substitute “.o” with “.d”

#############################################################

.PHONY : all clean

CS 343 – Assignment 4
	

9

ifeq (${shell if [ “${LOCKVIMPL}” = “${VIMPL}” –a “${OUTPUTTYPE}” = “${OUTPUT}” –a \

This makefile is invoked as follows:

    make vote VIMPL=MC BCHECK=BARGINGCHECK

    vote . . .

    make vote VIMPL=SEM OUTPUT=OUTPUT

    vote . . .

    make vote VIMPL=BAR OUTPUT=NOOUTPUT

    vote . . .

Put this Makefile in the directory with the programs, name the source files as specified above, and enter the appropriate make to compile a specific version of the programs. This Makefile must be submitted with the assignment to build the program, so it must be correct. Use the web tool Request Test Compilation to ensure you have submitted the appropriate files, your makefile is correct, and your code compiles in the testing environment.

Follow these guidelines. Your grade depends on it!
