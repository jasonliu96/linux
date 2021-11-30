### Questions ####
#### Q1 ####
I worked alone on this assignment // research required involved looking up how to call cpuid and rdtsc 
#### Q2 ####
#### Set Up ####
* To run the test code of the program we needed to be inside a nested VM 
* Using sudo apt-get install virt-manager// I was able to install virt manager a VMM to run my ubuntu machine v. 20.04.3
* Run necessary steps to install ubuntu 
* Run package installation for: gcc, cpuid
#### Leaf 0x4fffffff ####
* For this leaf I followed the demo video closely 
* Locate the files needed to be changed 
*  cpuid.c located in linux/arch/x86/kvm
*  vmx.c located in linux/arch/x86/kvm/vmx 
*  in cpuid.c locate the kvm_emulate_cpuid method located near the bottom of the file 
*  create a global variable u32 total_exits in cpuid.c 
*  use EXPORT_SYMBOL(total_exits) so that the global variable can be accessed in vmx.c
*  Create if statement so that if value of eax matches the leaf then return desired value
*  change vmx.c locate the __vmx_handle_exits method
*  use extern total_exits to load variable
*  Increment global variable total_exits within the method. 
#### Leaf 0x4ffffffe ####
* Locate the files needed to be changed 
*  cpuid.c located in linux/arch/x86/kvm
*  vmx.c located in linux/arch/x86/kvm/vmx 
* in cpuid.c create global variable u64 total_cycles
* use EXPORT_SYMBOL(total_cycles) so that vmx.c has access
* in vmx.c use extern total_cycles to gain access to the variable
* in vmx.c locate vmx_handle_exit 
* create a u64 variable to hold the start value obtained by calling rdtsc()
* turns out the linux/kvm code has a package with this method that can be called to return the needed value 
* I referenced this post https://stackoverflow.com/questions/13772567/how-to-get-the-cpu-cycle-count-in-x86-64-from-c/51907627#51907627 for information regarding rdtsc 
* initialize int ret; at the top of method or an error will be thrown; 
* call start = rdtsc() before the vmx_handle_exit() method calls __vmx_handle_exit() 
* call rdtsc() again after the method and incremenet the global variable 
* back in the cpuid.c method -> use (u32)(total_cycles>>32) and (u32)total_cycles to unpack the u64 total_cycles into ebx and ecx  
#### Loading Modules ####
* use command: make -j 7 modules //this builds the modules
* use command: sudo make INSTALL_MOD_STRIP=1 modules_install 
* use command: sudo reboot // this step here might be unneeded but my inner VM was not registering module updates without this step 
* run test file inside the nested vm 


*Added 11/39/2021*
#### Output 
Edits to the previous code include changing globals to their atomic counterparts. 
![image](https://user-images.githubusercontent.com/11416306/143965414-1ddf8c65-ed50-4a06-9875-f412d597ba79.png)

