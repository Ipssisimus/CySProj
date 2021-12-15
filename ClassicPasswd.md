https://tryhackme.com/room/classicpasswd

#############################################################

This is a walkthrough using Ghidra ( pre-installed with Kali )

Download the file Challenge.Challenge

Open it up with Ghidra in a new project and double click the file and analyze with decompiler box checked











void vuln(void)

{
  int iVar1;
  char local_2c8 [130];
  undefined8 local_246;
  undefined4 local_23e;
  undefined2 local_23a;
  char local_238 [512];
  undefined8 local_38;
  undefined8 local_30;
  undefined8 local_28;
  undefined2 local_20;
  undefined local_1e;
  undefined8 local_15;
  undefined4 local_d;
  undefined local_9;
  
  local_15 = 0x207962206564614d;
  local_d = 0x6e6f6e34;
  local_9 = 0;
  local_38 = 0x2f2f3a7370747468;
  local_30 = 0x632e627568746967;
  local_28 = 0x69626f306e2f6d6f;
  local_20 = 0x3474;
  local_1e = 0;
  local_246 = 0x6435736a36424741;
  local_23e = 0x476b6439;
  local_23a = 0x37;
  printf("Insert your username: ");
  __isoc99_scanf(&DAT_0010201b,local_238);
  strcpy(local_2c8,local_238);
  iVar1 = strcmp(local_2c8,(char *)&local_246);
  if (iVar1 == 0) {
    puts("\nWelcome");
    return;
  }
  puts("\nAuthentication Error");
                    /* WARNING: Subroutine does not return */
  exit(0);
}

