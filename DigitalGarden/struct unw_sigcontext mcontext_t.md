---
tags:
  - finished
---

/* On AArch64, we can directly use ucontext_t as the unwind context,
 * however, the __reserved struct is quite large: tune it down to only
 * the necessary used fields.  */

struct unw_sigcontext
  {
        uint64_t fault_address;
        uint64_t regs[31];
        uint64_t sp;
        uint64_t pc;
        uint64_t pstate;
        uint8_t __reserved[(34 * 8)] __attribute__((__aligned__(16)));
};

typedef struct
  {
        unsigned long uc_flags;
        struct ucontext *uc_link;
        stack_t uc_stack;
        sigset_t uc_sigmask;
        struct unw_sigcontext uc_mcontext;
  } unw_tdep_context_t;


typedef struct
  {
    unsigned long long int __ctx(fault_address);
    unsigned long long int __ctx(regs)[31];
    unsigned long long int __ctx(sp);
    unsigned long long int __ctx(pc);
    unsigned long long int __ctx(pstate);
    /* This field contains extension records for additional processor
       state such as the FP/SIMD state.  It has to match the definition
       of the corresponding field in the sigcontext struct, see the
       arch/arm64/include/uapi/asm/sigcontext.h linux header for details.  */
    unsigned char __reserved[4096] __attribute__ ((__aligned__ (16)));
  } mcontext_t;

[root@c54d59e438d1 zbs]# grep -r unw_tdep_context_t /usr/include/ 
/usr/include/libunwind-x86_64.h:typedef ucontext_t unw_tdep_context_t;
/usr/include/libunwind-x86_64.h:extern int unw_tdep_getcontext (unw_tdep_context_t *);
/usr/include/libunwind-common.h:typedef **unw_tdep_context_t unw_context_t;**

typedef struct ucontext_t
  {
    unsigned long int __ctx(uc_flags);
    struct ucontext_t *uc_link;
    stack_t uc_stack;
    mcontext_t uc_mcontext;
    sigset_t uc_sigmask;
    struct _libc_fpstate __fpregs_mem;
    __extension__ unsigned long long int __ssp[4];
  } ucontext_t;


## arm64
struct unw_sigcontext
  {
        uint64_t fault_address;
        uint64_t regs[31];
        uint64_t sp;
        uint64_t pc;
        uint64_t pstate;
        uint8_t __reserved[(34 * 8)] __attribute__((__aligned__(16)));
};

typedef struct
  {
        unsigned long uc_flags;
        struct ucontext *uc_link;
        stack_t uc_stack;
        sigset_t uc_sigmask;
        struct unw_sigcontext uc_mcontext;
  } unw_tdep_context_t;
  
## amd64
typedef struct
  {
    unsigned long long int __ctx(fault_address);
    unsigned long long int __ctx(regs)[31];
    unsigned long long int __ctx(sp);
    unsigned long long int __ctx(pc);
    unsigned long long int __ctx(pstate);
    /* This field contains extension records for additional processor
       state such as the FP/SIMD state.  It has to match the definition
       of the corresponding field in the sigcontext struct, see the
       arch/arm64/include/uapi/asm/sigcontext.h linux header for details.  */
    unsigned char __reserved[4096] __attribute__ ((__aligned__ (16)));
  } mcontext_t;