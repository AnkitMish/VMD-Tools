## VMD-Tools

# VMD Tools (2015.12.31)                                                
                                                                       
# -Functions-                                                           
  A. chview: Change Viewpoint by Moving Atomic Configurations          
  B. topdb: Export Selected Frames and Atoms as PDB a Trajectory File  
  C. pickconfig: Export Selected Frames as a Scaled Coordination File  
  D. ssr: Render Snapshots of Selected Frames                          
  E. makebonds: Make Bondlists for All Frames                          
  F. readbonds: Read Bondlist and Update Every Frames                  
  G. readdata: Read Trajectory Value as "User" Variable                
                                                                       

# Setup                                                                 
                                                                       
 1. Select "VMD Main -> Extansions -> TkConsole"                       
                                                                       
 2. Execute "source (path)/misawa.tcl"                                 
                                                                       
 3. Execute functions                                                  
                                                                         

## A. chview: Change Viewpoint by Moving Atomic Configurations           
                                                                       
 (How to use)                                                          
                                                                       
 chview -shift { $x $y $z }:                                           
   --- shift atomic positions in {$x $y $z} (ang.) in boundary box.    
       |$x|, |$y| and |$z| must be smaller than Lx, Ly and Lz          
                                                                       
 chview -com "$selection":                                             
   --- fit center of mass of $selection to center of view              
                                                                       
 chview -gc "$selection":                                              
   --- fit geometrical center of $selection to center of view          
                                                                       
 chview -reset                                                         
   --- reset view                                                      
                                                                       
 Example: chview -com "name O H"                                       
   --- fit center of mass of oxygen and hydrogen to center of view     
                                                                       
 Memo: Input lattice constant is required                              
       ("pbc set {$a $b $c $alpha $beta $gamma} -all" on Tk Console)   
       Only for orthorhombic cell                                      
                                                                       

## B. topdb: Export Selected Frames and Atoms as a PDB Trajectory File   
                                                                       
 (How to use)                                                          
                                                                       
  topdb $i $j $filename                                                
    $i: start frame                                                    
    $j: end frame                                                      
                                                                       
  topdb -skip $sk -sel $selection $i $j $filename                      
    $selsction: selection of atoms (like as "name Fe O")               
    $sk: skip frames                                                   
                                                                       
 Example: topdb -skip 5 0 100 trajectory.pdb                           
                                                                       

 C. pickconfig: Export Selected Frame as a Scaled Coordination File    
                                                                       
 (How to use)                                                          
  pickconfig $frame $filename                                          
    $frame: default frame is "now"                                     
    $filename: default filename is "Config.dat"                        
                                                                       
 Memo: Input lattice constant is required                              
       ("pbc set {$a $b $c $alpha $beta $gamma} -all" on Tk Console)   
       Only for orthorhombic cell                                      
                                                                       

## D. ssr: Render Snapshots of Selected Frames                           
                                                                       
 (How to use)                                                          
                                                                       
  ssr -frames $i $j                                                    
    $i: initial frame number                                           
    $j: final frame number                                             
                                                                       
  ssr -frame $i                                                        
    $i: render frame                                                   
                                                                       
  (other options)                                                      
  ssr -frame $i -form $text -skip $s -rend $r                          
    $text: format type (default: tga)                                  
    $s: skip every $s frame (default: 1)                               
    $rend: render type (default: 0)                                    
           $r = 0: Snapshots                                           
           $r = 1: Internal Tachyon                                    
                                                                       
 Example: ssr -frames 0 100 -rend TachyonInternal                      
   --- Frame000.tga ~ Frame100.tga will be created by Tachyon          
                                                                       

##  E. makebonds: Make Bondlists for All Frames                          
                                                                       
 (How to use)                                                          
                                                                       
  makebonds $filename                                                  
    (default filename is "./bondlist.dat")                             
    then enter the cutoff distances in ang.                            
                                                                       
  makebonds -pbc $filename                                             
    --- make bondlist with considering PBC                             
        (please execute "readbonds" with "-pbc" option                 
         when you apply a bondlist created with this option)           
                                                                       
  note: cutoff distance should be less than 3.0 ang.                   
                                                                       
  bondlist format:                                                     
  ---------------------                                                
  3 5 10     neighbors of atom 1, step 0                              
             neighbors of atom 2, step 0 (if no neighbors: empty)     
  1 12 100   neighbors of atom 3, step 0                              
   .                                                                   
   .                                                                   
   .                                                                    
  30 40 50   neighbors of atom n, step 0                              
  3 5 10 12  neighbors of atom 1, step 1                              
  3          neighbors of atom 1, step 1                              
   .                                                                   
   .                                                                   
   .                                                                   
                                                                       
 Memo: Input lattice constant is required                              
       ("pbc set {$a $b $c $alpha $beta $gamma} -all" on Tk Console)   
       Only for orthorhombic cell                                      
       VMD will be stop if there are too many atoms or frames          
       Create bondlists on other workspace (not on VMD) is recommended 
                                                                       

##  F. readbonds: Read Bondlist and Update Every Frames                  
                                                                       
 (How to use)                                                          
                                                                       
  readbonds $filename                                                  
    default filename is "./bondlist.dat"                               
                                                                       
  readbonds -pbc $filename                                             
    --- remove PBC bond with keeping "numbonds"                        
        if bond distance > 3 ang., it will be considered as PBC bond   
                                                                       
  Bondlist is available only for "Bonds" or "CPK" in Representation.   
                                                                       
  The bondlist is available only for current box.                      
  (if "pbc wrap -shiftcenter" or "chview" is done after readbonds,     
   "Bonds" or "CPK" will not represent exactlly.)                                 
                                                                       
  bondlist format:                                                     
  ---------------------                                                
  3 5 10     neighbors of atom 1, step 0                              
             neighbors of atom 2, step 0 (if no neighbors: empty)     
  1 12 100   neighbors of atom 3, step 0                              
   .                                                                   
   .                                                                   
   .                                                                    
  30 40 50   neighbors of atom n, step 0                              
  3 5 10 12  neighbors of atom 1, step 1                              
  3          neighbors of atom 1, step 1                              
   .                                                                   
   .                                                                   
   .                                                                   
                                                                       
 Memo: Input lattice constant is required                              
       ("pbc set {$a $b $c $alpha $beta $gamma} -all" on Tk Console)   
       Only for orthorhombic cell                                      
                                                                       

##  G. readdata: Read Trajectory Value as "User" Variable                
                                                                       
 (How to use)                                                          
                                                                       
  readdata $filename -str $i $j -var $var                              
    $filename: path of datafile                                        
    $i, $j: data strength (default: $i = 4, $j = 6)                    
    $var: variable name (default: user) e.g. user, user2, user3, ...   
                                                                       
  example:                                                             
    readdata ./coordination.dat -var use2                              
                                                                       
  datafile format:                                                     
  ---------------------                                                
  DATA 1.0   value for atom 1, step 0                                 
  DATA 2.0   value for atom 2, step 0                                 
  DATA 1.5   value for atom 3, step 0                                 
   .                                                                   
   .                                                                   
   .                                                                    
  DATA 1.2   value for atom n, step 0                                 
  END                                                                  
  DATA 1.0   value for atom 1, step 1                                 
  DATA 2.0   value for atom 2, step 1                                 
   .                                                                   
   .                                                                   
   .                                                                   
