# Questions 
## Q1 
I worked alone on this project. 
## Q2 
#### with EPT 
*full output listed below* \
Number of Exits: 3965260	Number of Cycles: 59111170947\
Exit 0 Value in eax register is: 26853\
Exit 1 Value in eax register is: 87495\
Exit 7 Value in eax register is: 24677\
Exit 10 Value in eax register is: 415134\
Exit 12 Value in eax register is: 254590\
Exit 28 Value in eax register is: 64908\
Exit 29 Value in eax register is: 25\
Exit 30 Value in eax register is: 577580\
Exit 31 Value in eax register is: 5325\
Exit 32 Value in eax register is: 474599\
Exit 40 Value in eax register is: 21477\
Exit 46 Value in eax register is: 129\
Exit 47 Value in eax register is: 63\
Exit 48 Value in eax register is: 1944236\
Exit 49 Value in eax register is: 71709\
Exit 54 Value in eax register is: 27\
Exit 55 Value in eax register is: 27

#### Without EPT 
Number of Exits: 13773494	Number of Cycles: 173149593545\
Exit 0 Value in eax register is: 1413415\
Exit 1 Value in eax register is: 425248\
Exit 7 Value in eax register is: 67130\
Exit 10 Value in eax register is: 553591\
Exit 12 Value in eax register is: 328866\
Exit 14 Value in eax register is: 180800\
Exit 28 Value in eax register is: 7218205\
Exit 29 Value in eax register is: 33\
Exit 30 Value in eax register is: 732116\
Exit 31 Value in eax register is: 7308\
Exit 32 Value in eax register is: 680552\
Exit 40 Value in eax register is: 30098\
Exit 46 Value in eax register is: 172\
Exit 47 Value in eax register is: 84\
Exit 48 Value in eax register is: 1955089\
Exit 49 Value in eax register is: 72761\
Exit 54 Value in eax register is: 31\
Exit 55 Value in eax register is: 36\
Exit 58 Value in eax register is: 147223

## Q3
There was a an increase in exit counts by a bit over a factor of 3; the number of cycles also increased by around a factor of 3. This was expected since the 'ept=0' operation forces the inner vm to operate on shadow paging instead of nested paging. Shadow paging is also associated with a higher overhead so a direct translation to more exits makes sense. 

## Q4
Ept = 0 sets the number of extended page tables to zero, this means that instead of nested paging the use of shadow paging is forced. Shadow paging uses exits for tlb flush as well as page faults and due to the number of required hits between the virtual page table, shadow page table, and tlb this leads to much more oportunities for exits to occur. 
The visible changes besides the increase in exits between ept vs no-ept is that for the no-ept there are also exits for code 58 and 14.  
Exit Code 14: INVLPG - invalid TLB entries 
Exit Code 58: INVPIC - Invalidate Process-Context Identifier
Since we are no longer operating with nested page and switching to shadow paging it appears that the vmm uses the INVLPG exit to mark any entries for memory locations in the active table as not present. 


#### Full Output 
#### with EPT 
Number of Exits: 3965260	Number of Cycles: 59111170947\
Exit 0 Value in eax register is: 26853\
Exit 1 Value in eax register is: 87495\
Exit 2 Value in eax register is: 0\
Exit 3 Value in eax register is: 0\
Exit 4 Value in eax register is: 0\
Exit 5 Value in eax register is: 0\
Exit 6 Value in eax register is: 0\
Exit 7 Value in eax register is: 24677\
Exit 8 Value in eax register is: 0\
Exit 9 Value in eax register is: 0\
Exit 10 Value in eax register is: 415134\
Exit 11 Value in eax register is: 0\
Exit 12 Value in eax register is: 254590\
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
Exit 28 Value in eax register is: 64908\
Exit 29 Value in eax register is: 25\
Exit 30 Value in eax register is: 577580\
Exit 31 Value in eax register is: 5325\
Exit 32 Value in eax register is: 474599\
Exit 33 Value in eax register is: 0\
Exit 34 Value in eax register is: 0\
Exit 35 Value in eax register is: 0\
Exit 36 Value in eax register is: 0\
Exit 37 Value in eax register is: 0\
Exit 38 Value in eax register is: 0\
Exit 39 Value in eax register is: 0\
Exit 40 Value in eax register is: 21477\
Exit 41 Value in eax register is: 0\
Exit 42 Value in eax register is: 0\
Exit 43 Value in eax register is: 0\
Exit 44 Value in eax register is: 0\
Exit 45 Value in eax register is: 0\
Exit 46 Value in eax register is: 129\
Exit 47 Value in eax register is: 63\
Exit 48 Value in eax register is: 1944236\
Exit 49 Value in eax register is: 71709\
Exit 50 Value in eax register is: 0\
Exit 51 Value in eax register is: 0\
Exit 52 Value in eax register is: 0\
Exit 53 Value in eax register is: 0\
Exit 54 Value in eax register is: 27\
Exit 55 Value in eax register is: 27\
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

#### Without EPT 
Number of Exits: 13773494	Number of Cycles: 173149593545\
Exit 0 Value in eax register is: 1413415\
Exit 1 Value in eax register is: 425248\
Exit 2 Value in eax register is: 0\
Exit 3 Value in eax register is: 0\
Exit 4 Value in eax register is: 0\
Exit 5 Value in eax register is: 0\
Exit 6 Value in eax register is: 0\
Exit 7 Value in eax register is: 67130\
Exit 8 Value in eax register is: 0\
Exit 9 Value in eax register is: 0\
Exit 10 Value in eax register is: 553591\
Exit 11 Value in eax register is: 0\
Exit 12 Value in eax register is: 328866\
Exit 13 Value in eax register is: 0\
Exit 14 Value in eax register is: 180800\
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
Exit 28 Value in eax register is: 7218205\
Exit 29 Value in eax register is: 33\
Exit 30 Value in eax register is: 732116\
Exit 31 Value in eax register is: 7308\
Exit 32 Value in eax register is: 680552\
Exit 33 Value in eax register is: 0\
Exit 34 Value in eax register is: 0\
Exit 35 Value in eax register is: 0\
Exit 36 Value in eax register is: 0\
Exit 37 Value in eax register is: 0\
Exit 38 Value in eax register is: 0\
Exit 39 Value in eax register is: 0\
Exit 40 Value in eax register is: 30098\
Exit 41 Value in eax register is: 0\
Exit 42 Value in eax register is: 0\
Exit 43 Value in eax register is: 0\
Exit 44 Value in eax register is: 0\
Exit 45 Value in eax register is: 0\
Exit 46 Value in eax register is: 172\
Exit 47 Value in eax register is: 84\
Exit 48 Value in eax register is: 1955089\
Exit 49 Value in eax register is: 72761\
Exit 50 Value in eax register is: 0\
Exit 51 Value in eax register is: 0\
Exit 52 Value in eax register is: 0\
Exit 53 Value in eax register is: 0\
Exit 54 Value in eax register is: 31\
Exit 55 Value in eax register is: 36\
Exit 56 Value in eax register is: 0\
Exit 57 Value in eax register is: 0\
Exit 58 Value in eax register is: 147223\
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
