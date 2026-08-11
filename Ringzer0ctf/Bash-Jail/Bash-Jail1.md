# Synopsis
                                                                                                                                                                                                                                           
BASH Jail Level 1:                                                                                                                                                                                                                         
Current user is uid=2001(level1) gid=2001(level1) groups=2001(level1),1000(challenger)                                                                                                                                                     
                                                                                                                                                                                                                                           
Flag is located at /home/level1/flag.txt                                                                                                                                                                                                   
                                                                                                                                                                                                                                           
Challenge bash code:                                                                                                                                                                                                                       
-----------------------------                                                                                                                                                                                                              
                                                                                                                                                                                                                                           
echo                                                                                                                                                                                                                                       
echo "Flag is located at $(pwd)/flag.txt"                                                                                                                                                                                                  
echo                                                                                                                                                                                                                                       
echo "Challenge bash code:"                                                                                                                                                                                                                
echo "-----------------------------"                                                                                                                                                                                                       
echo -e '\033[0;31m'                                                                                                                                                                                                                       
sed -e '1,19d' < $0                                                                                                                                                                                                                        
echo -e '\033[0m'                                                                                                                                                                                                                          
echo "-----------------------------"                                                                                                                                                                                                       
                                                                                                                                                                                                                                           
# CHALLENGE                                                                                                                                                                                                                                
                                                                                                                                                                                                                                           
while :                                                                                                                                                                                                                                    
do                                                                                                                                                                                                                                         
        echo "Your input:"                                                                                                                                                                                                                 
        read input                                                                                                                                                                                                                         
        output=`$input`                                                                                                                                                                                                                    
done                                                                                                                                                                                                                                       

# Method of Solve
**cp flag.txt /dev/stderr**
this works as it invokes 2 plain literal arguments - flag,txt and /dev/stderr. 
Word-splitting alone is enough to produce the right argument vector.
**FLAG-U96l4k6m72a051GgE5EN0rA85499172K**

          
