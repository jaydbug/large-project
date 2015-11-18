/* Jayd Hill, Cathy Webster, Kathryn Lovett
 * CIS 314 Fall 2015 Large Project
 * 
 * Single cylce processor simulation in C running
 * MIPS assembly language with a main memory unit.
 * Read in up to two files from the command line.
*/

 #include <stdio.h>
 #include <stdlib.h>
 #include <string.h>

 void parser(char *getOther[100], char *lab[100], char *getInstruct[100], int i);
 void inti_registers(char *reg_file);
 void DECODE(char instruct);
 
struct Registers
{
   char _name[4];
   int val;
};
struct Registers reg_file[32];

struct Memory_Data
{
   char _name[10];
   int val;

};
struct Memory_Data mem_array[];



//Read arguments from command line
 int main(int argc, char *argv[]){
   if( argc == 2 ) {
      printf("The argument supplied is %s\n", argv[1]);
   }
   else if( argc > 2 ) {
      printf("Too many arguments supplied.\n");
   }
   else {
      printf("One argument expected.\n");
   }

   FILE *in_file = fopen(argv[1], "r");


   //Read in instructions line by line and save to instruction array
   char *getOther[100];
   char *labels[100];
   char *getInstruct[100];
   char line[100];
   

   int i = 0;
   while(fgets(line, 100, in_file) != NULL) {
      if(line[0] != '#' && line[0] != '\n' && strlen(line) > 2) {
         int j;
         for(j = 0; j < sizeof(line); j++) {
            if(line[j] == ('\t')){
               line[j] = ' ';
            }
         }
         getOther[i] = malloc(strlen(line));
         labels[i] = malloc(strlen(line));
         getInstruct[i] = malloc(strlen(line));
         strcpy(getOther[i], line);
         strcpy(labels[i], line);
         strcpy(getInstruct[i], line);
         i++;
      }
      
   }
   /*
   for(int x = 0; x < i; x++) {
      printf("\n%s", instructions[x]);
   }
   */
   parser(getOther, labels, getInstruct, i);
   
   int a = 0;
   while(getOther[a] != NULL) {
      free(getOther[a]);
      a++;
   }
   

   fclose(in_file);
   return 0;
 }

 void parser(char *getOther[100], char *lab[100], char *getInstruct[100], int i) {
   char *LABEL[10];   
   int LABELINDEX[10];
   char *INSTRUCTIONARRAY[100];
   char *tempInstruct;
   char *allInstruct[50];
   char *tempArr;
   char *tempOther;
   char *allOther[50];

   int r = 0;
   for(int p = 0; p < i; p++) {
      tempArr = strtok(lab[p], " ");

      if(tempArr == NULL || strlen(tempArr) < 2) {
         continue;
      }

      for(int q = 0; tempArr[q] != NULL; q++) {
         if(tempArr[q] == ':') {
            LABEL[r] = malloc(strlen(strtok(tempArr, ":")));
            strcpy(LABEL[r], strtok(tempArr, ":"));
            LABELINDEX[r] = p;
            r++;
         }
      }
   }

   for(int p = 0; p < r; p++) {
      printf("Label: %s Line: %d\n", LABEL[p], LABELINDEX[p]);
   }

   int s = 0;
   for(int p = 0; p < i; p++) {
      tempInstruct = strtok(getInstruct[p], " ");

      for(int q = 0; tempInstruct[q] != NULL; q++) {
         if(tempInstruct[q] == ':') {
            strcpy(tempInstruct, "");
         }
      }
      allInstruct[s] = malloc(strlen(tempInstruct));
      strcpy(allInstruct[s], tempInstruct);
      s++;
   }

   for(int p = 0; p < i; p++) {
      printf("%s\n", allInstruct[p]);
   }

   int u = 0;
   for(int p = 0; p < i; p++) {
      getOther[p] = strtok(getOther[p], "#");

      //Remove instructions and labels
      tempOther = strtok(getOther[p], " ");
      printf("Token is %s\n", tempOther);
      tempOther = strtok(NULL, " ");
      printf("Token is %s\n", tempOther);
      allOther[u] = malloc(strlen(tempOther));
      strcpy(allOther[u], tempOther);
      printf("Registers are: %s\n", allOther[u]);
      u++;
      /*
      for(int t = 0; tempOther != NULL; t++) {
         printf("token %d is %s\n", t, tempOther);
         tempOther = strtok(NULL, " ");
      }*/
   }

   for(int p = 0; p < i; p++) {
      printf("%s\n", allOther[p]);
   }
 }



// UPDATED FROM HERE!!!!!!!!!!!!!!!!!!!!
void inti_registers(char *reg_file) {
   int i;

   char * reg_names[] = {"$at","$v0","$v1","$a0","$a1","$a2","$a3","$t0","$t1","$t2","$t3","$t4","$t5","$t6","$t7",
   "$s0","$s1","$s2","$s3","$s4","$s5","$s6","$s7","$t8","$t9","$k0","$k1","$gp","$sp","$fp","$ra"};

// all registers are intially zero
   for (i = 0; i < 32; i++) {
      reg_file[i].val = "0";
      strncpy(reg_file[i]._name, reg_names[i], 4);
   };
}

int searchPos(char *arr) {
   int x;
   for (x = 0; x < 32; x++) {
      if(strcmp(arr, reg_file[x]._name) == 0) {
         return x;
      }
   }}

int searchMemArray(char *arr) {
   int x;
   for (x = 0; x < 32; x++) {
      if (arr(&strcmp, mem_array[x].val) == 0) {
         return x;
      }
   }}

int searchLabel(char * arr) {
   int x;
   for (x = 0; x < 10; x++) {
      if (strcmp(arr, LABELINDEX[x]) == 0) {
         return x;
      }
   }}

//DECODE(instruct[i]) for i=0,...,sizeof(instruct)
void DECODE(char *instruct) {
   // instruct[a] = "add $t3, $t2, $t1"
   int a=0, b = 0, index = 0;
   char instr_arr[4];
   char var1[8];
   char var2[8];
   char var3[8];

   // save the instr
   while (instruct[a][index] != ' ') {
      instr_arr[index] = instr[a][index];
      index++;
   }
   index++;

// add, addi, sub, slt, sltu, mult
   // add, addi, sub, slt, sltu, mult
   int add_true = strcmp(instr_array, "add");
   int addi_true = strcmp(instr_array, "addi");
   int sub_true = strcmp(instr_array, "sub");
   int slt_true = strcmp(instr_array, "slt");
   int sltu_true = strcmp(instr_array, "sltu");
   int mult_true = strcmp(instr_array, "mult");
   
   if (add_true == 0 || addi_true == 0 || sub_true == 0 || slt || sltu || mult) {
      while (instruct[a][index] == ' ') {
         index++; // skip until the next char
      }
      while(instruct[a][index] != ',') {
         strcpy(var1[b], instruct[a][index]);
         b++;
         index++;
      }
      index = index + 2;
      b = 0;
      while(instruct[a][index] != ',') {
         strcopy(var2[b], instruct[a][index]);
         index++;
         b++;
      }}
      index = index + 2;
      b = 0;
      while(instruct[a][index] != '\0') {
         strcpy(var3[b], instruct[a][index]);
         index++;
         b++;
      }}}

// lw, sw
   int lw_true = strcmp(instr_array, "lw");
   int sw_true = strcmp(instr_array, "sw");

   else if (lw_true == 0 || sw_true == 0) {
      while (instruct[a][index] != " "){
         index++;
      }
      while ((instruct[a][index] != ','))
         strcpy(var1[b], instruct[a][index]);
         index++;
         b++;
      }
      index = index + 2;
      b = 0;
      while (instruct[a][index] != "("){
         strcpy(var2[b], instruct[a][index]);
         index++;
      }
      index++;
      b = 0;
      while (instruct[a][index] != ")" || instruct[a][index] != '\0') {
         strcpy(var3[b], instruct[a][index]);
      }
   }

//beq, bne
int beq_true = strcmp(instr_array, "beq");
int bne_true = strcmp(instr_array, "bne");
   
else if (beq_true == 0 || bne_true == 0) {
   while (instruct[a][index] == ' ') {
         index++; // skip until the next char
   }
   while(instruct[a][index] != ',') {
      var1[b] = instruct[0][index];
      b++;
      index++;
   }
   index = index + 2;
   b = 0;
   while(instruct[a][index] != ',') {
      strcpy(var2[b], instruct[a][index]);
      index++;
      b++;
   }
   index = index + 2;
   b = 0;
   while(instruct[a][index] != '\0') {
      strcpy(var3[b], instruct[a][index];
      index++;
      b++;
   }
}

//j, jal, jr
int j_true = strcmp(instr_array, "j");
int jal_true = strcmp(instr_array, "jal");
int jr_true = strcmp(instr_array, "jr");

else if (j_true == 0 || jal_true == 0 || jr_true == 0) {
   while(instr_arr[a][index] != "\0") {
      var1[b] = instr_arr[0][index];
      index++;
      b++;
   }
}

//EXECUTE(instr_arr, var1, var2, var3);
      
}

//EXECUTE!!!!!!!!!!!!!!!!!!
// do the actual decoding and calculating
void EXECUTE(char instr_arr, char var1, char var2, char var3){
   if(strcmp((instr_array, "add")==0)){
      int a = searchPos(var1);
      int b = searchPos(var2);
      int c = searchPos(var3);
      add(a, b, c);
   }

   else if(strcmp((instr_array, "addi")==0)){
      int a = searchPos(var1);
      int b = searchPos(var2);
      int c = atoi(var3)
      addi(a, b, c);
   } 

   else if(strcmp((instr_array, "sub")==0)){
      int a = searchPos(var1);
      int b = searchPos(var2);
      int c = searchPos(var3);
      sub(a, b, c);
   }
   
   else if(strcmp((instr_array, "mult")==0)){
      int a = searchPos(var1);
      int b = searchPos(var2);
      int c = searchPos(var3);
      mult(a, b, c);
   }

   else if(strcmp((instr_array, "slt")==0)){
      int a = searchPos(var1);
      int b = searchPos(var2);
      int c = searchPos(var3);
      slt(a, b, c);
   }
   
   else if(strcmp((instr_array, "sltu")==0)){
      int a = searchPos(var1);
      int b = searchPos(var2);
      int c = searchPos(var3);
      sltu(a, b, c);
   }

   //lw
   else if (strcmp(instr_arr, "lw") == 0) {
      int a = searchPos(var1);
      int b = atoi(var2);
      int c = reg_file[searchPos(var3)].val;
      c = (b + c) / 4;
      int data = mem_array[c];
      lw(a, data);
   }

   //sw
   else if (strcmp(instr_arr, "sw") == 0) {
      int a = reg_file[searchPos(var1)].val;
      int b = atoi(var2);
      int c = reg_file[searchPos(var3)].val;
      c = (b + c) / 4 ;
      sw(c, a);

   }

   //beq
   else if (strcmp(instr_arr, "beq") == 0) {
      //save the index
      int dest_pos = searchLabel(var3);
      int one = searchPos(var1);
      int two = searchPos(var2);
      beq(dest_pos, one, two);
   }

   else if (strcmp(instr_arr, "bne") == 0) {
      int dest_pos = searchLabel(var3);
      int one = searchPos(var1);
      int two = searchPos(var2);
      bne(dest_pos, one, two);
   }

   else if (strcmp(instr_arr, "j") == 0) {
      int dest_pos = searchLabel(var1);
      j(dest_pos);
   }
   
   else if (strcmp(instr_arr, "jal") == 0) {
      int dest_pos = searchLabel(var1);
      jal(dest_pos);
   }
   else if (strcmp(instr_arr, "jr") == 0) {
      int dest_pos = searchLabel(var1);
      jr(dest_pos);
   }
}

// CALL MEMORY

// WRITE BACK TO REGISTER

//calling line counter pc (taken from MIPS instructions)
void add(int res, int r1, int r2){
   int a = reg_file[r1].val;
   int b = reg_file[r2].val;
   reg_file[res].val = a + b;
   pc++;
   return;
}

void addi(int res, int r1, int num){
   int a = reg_file[r1].val;
   reg_file[res].val = num + a;
   pc++;
   return;
}

void sub(int res, int r1, int r2){
   int a = reg_file[r1].val;
   int b = reg_file[r2].val;
   reg_file[res].val = b - a;
   pc++;
   return;
}

void mult(int res, int r1, int r2){
   int a = reg_file[r1];
   int b = reg_file[r2];
   reg_file[res].val = b*a;
   pc++;
   return;
}

void slt(int res, int r1, int r2){
   int a = reg_file[r1];
   int b = reg_file[r2];
   if(a < b){
      reg_file[res].val = 1;
   }
   else{
      reg_file[res].val = 0;
   }
   pc++;
   return;
}

void sltu(int res, int r1, int r2){
   int a = reg_file[r1];
   int b = reg_file[r2];
   if(a < b){
      reg_file[res].val = 1;
   }
   else{
      reg_file[res].val = 0;
   }
   pc++;
   return;
}

void lw(int _address, int data) {
   reg_file[_address].val = data;
   pc++;
}

void sw(int _address, int data) {
   mem_array[_address].val = data;
   pc++;
}

void beq(int dest, int r1, int r2) {
   int a = reg_file[r1];
   int b = reg_file[r2];
   if(a == b){
      pc = LABELINDEX[dest];
   }
   else{
      pc++;
   }
   return;
}

void bne(int dest, int r1, int r2){
   int a = reg_file[r1];
   int b = reg_file[r2];
   if(a != b){
      pc = LABELINDEX[dest];
   }
   else{
      pc++;
   }
   return;
}

void j(int dest){
   pc = LABELINDEX[dest];
   return;
}

void jal(int dest){
   reg_file[31].val = CURRENT LINE (pc or whatever);
   pc = LABELINDEX[dest];
   return;
}

void jr(int r1){
   pc = reg_file[r1].val;
   return;
}
