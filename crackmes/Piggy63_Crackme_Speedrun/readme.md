# Format for ReadMes

## Table of Contents
* [Introduction](#introduction)
* [Running the program](#Running-the-program)
* [Ghidra Analysis](#Ghidra-Analysis)
    - [Final Run](#Final-Run)
* [Conclusion](#Conclusion)
## Introduction

## Running the program

## Ghidra-Analysis

The first step here is to find main, unfortunately, I was unable to find a main function by just searching, so I had to look into the 
entry function to see which function was being called first. That ended up being this function below.

```
int main(void)

{
  code *pcVar1;
  bool bVar2;
  bool bVar3;
  undefined4 uVar4;
  int iVar5;
  code **ppcVar6;
  uint *puVar7;
  int unaff_EBP;
  int unaff_ESI;
  undefined4 uVar8;
  undefined4 uVar9;
  
  FUN_00402060(&DAT_0041dd08,0x14);
  uVar4 = ___scrt_initialize_crt(1);
  if ((char)uVar4 != '\0') {
    bVar2 = false;
    *(undefined *)(unaff_EBP + -0x19) = 0;
    *(undefined4 *)(unaff_EBP + -4) = 0;
    uVar4 = ___scrt_acquire_startup_lock();
    *(char *)(unaff_EBP + -0x24) = (char)uVar4;
    if (DAT_0041fca4 != 1) {
      if (DAT_0041fca4 == 0) {
        DAT_0041fca4 = 1;
        iVar5 = __initterm_e((undefined **)&DAT_00418124,(undefined **)&DAT_0041813c);
        if (iVar5 != 0) {
          *(undefined4 *)(unaff_EBP + -4) = 0xfffffffe;
          unaff_ESI = 0xff;
          goto LAB_0040198c;
        }
        __initterm((undefined **)&DAT_00418118,(undefined **)&DAT_00418120);
        DAT_0041fca4 = 2;
      }
      else {
        bVar2 = true;
        *(undefined *)(unaff_EBP + -0x19) = 1;
      }
      ___scrt_release_startup_lock((char)*(undefined4 *)(unaff_EBP + -0x24));
      ppcVar6 = (code **)FUN_00401e2f();
      if (*ppcVar6 != (code *)0x0) {
        uVar4 = ___scrt_is_nonwritable_in_current_image();
        if ((char)uVar4 != '\0') {
          pcVar1 = *ppcVar6;
          uVar9 = 0;
          uVar8 = 2;
          uVar4 = 0;
          _guard_check_icall();
          (*pcVar1)(uVar4,uVar8,uVar9);
        }
      }
      puVar7 = (uint *)FUN_00401e35();
      if (*puVar7 != 0) {
        uVar4 = ___scrt_is_nonwritable_in_current_image();
        if ((char)uVar4 != '\0') {
          __register_thread_local_exe_atexit_callback(*puVar7);
        }
      }
      FUN_0040a2d1();
      FUN_0040a3b9();
      FUN_0040a3b3();
      unaff_ESI = FUN_00401600();
      bVar3 = is_managed_app();
      if (bVar3) {
        if (!bVar2) {
          __cexit();
        }
        ___scrt_uninitialize_crt('\x01','\0');
        *(undefined4 *)(unaff_EBP + -4) = 0xfffffffe;
LAB_0040198c:
        ExceptionList = *(void **)(unaff_EBP + -0x10);
        return unaff_ESI;
      }
      goto LAB_004019a3;
    }
  }
  ___scrt_fastfail(7);
LAB_004019a3:
  _exit(unaff_ESI);
  __exit(*(int *)(unaff_EBP + -0x20));
  pcVar1 = (code *)swi(3);
  iVar5 = (*pcVar1)();
  return iVar5;
}
```

Looking at this here, this si alot to take in but we can move through it slowly, step by step.
First thing I noticed is that main takes no arguments, so there are no command line arguments that could affect this program.


## Final-Run

## Conclusion