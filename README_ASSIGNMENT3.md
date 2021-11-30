## Questions
### Q1
I worked alone on this assignment 
* research required involved cross referencing KVM exits with SDM exits
* Swapped globals to atomic versions due to weird ouputs on certain ecx inputs
* reading through vmx.c code to figure out optimal rdstc() placements.  
### Q2
#### Setup
Setup was the same as in assignment 2, since the nested VM was already operational
#### Research
* Exit Codes not included in SDM, values < 0, values > 68, 35, 38, 42, 65
* Exit Codes in the SDM but not supported by KVM include 3, 4, 5, 6, 11, 16, 17, 33, 34, 51, 63, 64, 66, 67, 68
  * These were figured out by cross referencing exit_reason in (*kvm_vmx_exit_handlers[]) 

#### Leaf 0x4ffffffd
* Add in a global array holding atomic_t types which are 32 bit. 
* Export_global(atomic_t) variable and in the vmx.c file extern it. 
* The indexing for this array will be the error codes. 
* in the cpuid.c located the emulate_cpuid method at the bottom of the file 
* add in conditional statements checking if eax == 0x4ffffffd 
* add in conditional checks for ecx values, 
 ![image](https://user-images.githubusercontent.com/11416306/143964323-0d7ec970-0ae3-49a7-bec0-f12fffdb46d3.png)
* I used this method for both leaf values added in this assignment
![image](https://user-images.githubusercontent.com/11416306/143964403-27099ca7-cdad-4748-8616-886122d5c093.png)
* This was the code added for 0x4ffffffd leaf
* In the vmx.c file I added 
* ![image](https://user-images.githubusercontent.com/11416306/143964484-0259b651-ebbd-405b-9f78-f1324b2c88c7.png)
* Originally I had exit_handler_index as the array index, however this missed several of the exits that seemed like kvm would have
* I then swapped it over to exit_reason.basic
#### Leaf 0x4ffffffc
* Add in a global array holding atomic64_t types. 
* Export_global(atomic64_t) variable and in the vmx.c file extern it. 
* in the cpuid.c located the emulate_cpuid method at the bottom of the file 
* add in conditional statements checking if eax == 0x4ffffffc 
* add in conditional checks for ecx values, similar to the 0x4ffffffd leaf.
* ![image](https://user-images.githubusercontent.com/11416306/143964727-8032c58b-61b0-4f04-90e5-004f40df262d.png)
* This was the code I had for the 0x4ffffffc leaf 
* In the vmx.c file I added an rdtsc() call at the beginning of the __vmx_handle_exit method 
* Additionally after I incremement the array for exit_counts I add the delta with the index into an array with the cycles per exit. 
* ![image](https://user-images.githubusercontent.com/11416306/143964906-1e23bf43-da40-4dc1-8ef8-96f85603abd9.png)
#### Building the Module
* Run make modules -j 7 modules
* sudo make INSTALL_MOD_STRIP=1 modules_install
* sudo reboot
* Enter Nested VM run test program. 
#### OUTPUTS
##### 0x4ffffffc Leaf Output
Starting Leaf node = 0x4ffffffc\
Exit 0 Value in ebx: 0	  ecx: 1920914	Number of Cycles: 1920914\
Exit 1 Value in ebx: 0	  ecx: 5601330	Number of Cycles: 5601330\
Exit 2 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 3 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 4 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 5 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 6 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 7 Value in ebx: 0	  ecx: 1058866	Number of Cycles: 1058866\
Exit 8 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 9 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 10 Value in ebx: 0	  ecx: 26843236	Number of Cycles: 26843236\
Exit 11 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 12 Value in ebx: 0	  ecx: 15585026	Number of Cycles: 15585026\
Exit 13 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 14 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 15 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 16 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 17 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 18 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 19 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 20 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 21 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 22 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 23 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 24 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 25 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 26 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 27 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 28 Value in ebx: 0	  ecx: 16074354	Number of Cycles: 16074354\
Exit 29 Value in ebx: 0	  ecx: 1666	Number of Cycles: 1666\
Exit 30 Value in ebx: 0	  ecx: 200592032	Number of Cycles: 200592032\
Exit 31 Value in ebx: 0	  ecx: 531270	Number of Cycles: 531270\
Exit 32 Value in ebx: 0	  ecx: 30342930	Number of Cycles: 30342930\
Exit 33 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 34 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 35 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 36 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 37 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 38 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 39 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 40 Value in ebx: 0	  ecx: 1170288	Number of Cycles: 1170288\
Exit 41 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 42 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 43 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 44 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 45 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 46 Value in ebx: 0	  ecx: 6252	Number of Cycles: 6252\
Exit 47 Value in ebx: 0	  ecx: 2848	Number of Cycles: 2848\
Exit 48 Value in ebx: 0	  ecx: 113218371	Number of Cycles: 113218371\
Exit 49 Value in ebx: 0	  ecx: 5074234	Number of Cycles: 5074234\
Exit 50 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 51 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 52 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 53 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 54 Value in ebx: 0	  ecx: 2366	Number of Cycles: 2366\
Exit 55 Value in ebx: 0	  ecx: 1346	Number of Cycles: 1346\
Exit 56 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 57 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 58 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 59 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 60 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 61 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 62 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 63 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 64 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 65 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 66 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 67 Value in ebx: 0	  ecx: 0	Number of Cycles: 0\
Exit 68 Value in ebx: 0	  ecx: 0	Number of Cycles: 0

##### 0x4fffffd Leaf Output 
Starting Leaf node = 0x4ffffffd\
Exit 0 Value in eax register is: 17894\
Exit 1 Value in eax register is: 29835\
Exit 2 Value in eax register is: 0\
Exit 3 Value in eax register is: 0\
Exit 4 Value in eax register is: 0\
Exit 5 Value in eax register is: 0\
Exit 6 Value in eax register is: 0\
Exit 7 Value in eax register is: 9843\
Exit 8 Value in eax register is: 0\
Exit 9 Value in eax register is: 0\
Exit 10 Value in eax register is: 358160\
Exit 11 Value in eax register is: 0\
Exit 12 Value in eax register is: 141748\
Exit 13 Value in eax register is: 0\
Exit 14 Value in eax register is: 0\
Exit 15 Value in eax register is: 0\
Exit 16 Value in eax register is: 0\
Exit 17 Value in eax register is: 0\
Exit 18 Value in eax register is: 0\
Exit 19 Value in eax register is: 0\
Exit 20 Value in eax register is: 0\
Exit 21 Value in eax register is: 0\
Exit 22 Value in eax register is: 0\
Exit 23 Value in eax register is: 0\
Exit 24 Value in eax register is: 0\
Exit 25 Value in eax register is: 0\
Exit 26 Value in eax register is: 0\
Exit 27 Value in eax register is: 0\
Exit 28 Value in eax register is: 226744\
Exit 29 Value in eax register is: 16\
Exit 30 Value in eax register is: 2222594\
Exit 31 Value in eax register is: 5754\
Exit 32 Value in eax register is: 273051\
Exit 33 Value in eax register is: 0\
Exit 34 Value in eax register is: 0\
Exit 35 Value in eax register is: 0\
Exit 36 Value in eax register is: 0\
Exit 37 Value in eax register is: 0\
Exit 38 Value in eax register is: 0\
Exit 39 Value in eax register is: 0\
Exit 40 Value in eax register is: 9868\
Exit 41 Value in eax register is: 0\
Exit 42 Value in eax register is: 0\
Exit 43 Value in eax register is: 0\
Exit 44 Value in eax register is: 0\
Exit 45 Value in eax register is: 0\
Exit 46 Value in eax register is: 85\
Exit 47 Value in eax register is: 42\
Exit 48 Value in eax register is: 1220821\
Exit 49 Value in eax register is: 54591\
Exit 50 Value in eax register is: 0\
Exit 51 Value in eax register is: 0\
Exit 52 Value in eax register is: 0\
Exit 53 Value in eax register is: 0\
Exit 54 Value in eax register is: 18\
Exit 55 Value in eax register is: 18\
Exit 56 Value in eax register is: 0\
Exit 57 Value in eax register is: 0\
Exit 58 Value in eax register is: 0\
Exit 59 Value in eax register is: 0\
Exit 60 Value in eax register is: 0\
Exit 61 Value in eax register is: 0\
Exit 62 Value in eax register is: 0\
Exit 63 Value in eax register is: 0\
Exit 64 Value in eax register is: 0\
Exit 65 Value in eax register is: 0\
Exit 66 Value in eax register is: 0\
Exit 67 Value in eax register is: 0\
Exit 68 Value in eax register is: 0

#### Issues Encountered
* for some of the exit types eax was returning huge overflows causing the number to turn negative // fixed after switching to atomic globals
* ecx input was in hex, but for my comparison for exit codes not supported/not listed in SDM i was using decimal //fixed by changing to hex
* using exit_handler_index resulted in different results than exit_reason.basic 
  * exit_reason.basic had outputs for error code 1 whereas exit_handler_index did not. 
#### Interesting things to note
* For some exit types, if not supported by KVM and called, will output a negative constant in ecx. // after adding the ecx checks this disappears. 

### Q3
Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations?
For the most part it seemed that vm exits increased stably during normal operation, Besides from running the test code I didn't really have any extensive usage inside the nested vm. One thing to note though, is that over a period of time if just running the test file over and over again that exits 48 and 49 increase much more than all the other exits. After every test program run the CPUID exit which is number 10 also incrememnts in segments of around 100 which another frequent exit CR Access code 28 doesn't change. So its interesting to see that the cpuid exits actually correspond to actions operated within the inner vm. 
Approximately how many exits does a full VM boot entail?
On my system *ubuntu 20.04 running on virt manager inside another ubuntu 20.04 running on vmware* after a fresh reboot and starting the inner vm on virt manager I had around 4.4m exits with	51.4b cycles. 

### Q4
Of the exit types defined in the SDM, which are the most frequent? Least?
So it turns out that generally even with supported functionality a lot of the exits are never called during the boot cycle which makes sense. \
Of the ones that were called The Non-Zero exits included :
* 29 - 16 times - Mov DR
* 46 - 85 times - Access to GDTR
* 47 - 42 times - Accress to LDTR
* 54 - 18 times - WBINVD
* 55 - 18 times - XSETBV\
The ones that were called most frequently included: 
* 10 - 360k times - CPUID 
* 12 - 141k times - HLT
* 28 - 226k times - CR Access
* 30 - 2.2m times - i/o Instruction
* 32 - 372k times - WRMSR
* 48 - 1.2m times - EPT Violation
comments: CPUID exits incremement by around 100 after back to back calls to the test program which interally calls cpuid via inline asm calls. The high frequency of i/o calls might be because of of switching from the host machine/vm/nested vm, it might also be because of the hardware clock. and EPT Violation seems to happen when the nested vm touches specific parts of memory (not 100% sure about the last bit). 
