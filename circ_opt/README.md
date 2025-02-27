Basic option system for CirC.

## Tests ([`trycmd`](https://docs.rs/trycmd/latest/trycmd/index.html))

These tests are based on the example binary in `examples/parser.rs`.
It contains a `clap::Parser` that simply includes `CircOpt`:
```rust
use clap::Parser;
use circ_opt::CircOpt;

#[derive(Parser, Debug)]
struct BinaryOpt {
    #[command(flatten)]
    pub circ: CircOpt,
}

fn main() {
    let opt = BinaryOpt::parse();
    println!("{:#?}", opt);
}
```

### Help Messages
```console
$ parser --help
? 0
Options that configure CirC

Usage: parser [OPTIONS]

Options:
      --r1cs-verified <VERIFIED>
          Use the verified field-blaster
          
          [env: R1CS_VERIFIED=]
          [default: false]
          [possible values: true, false]

      --r1cs-div-by-zero <DIV_BY_ZERO>
          Which field division-by-zero semantics to encode in R1cs
          
          [env: R1CS_DIV_BY_ZERO=]
          [default: incomplete]

          Possible values:
          - incomplete: Division-by-zero renders the circuit incomplete
          - zero:       Division-by-zero gives zero
          - non-det:    Division-by-zero gives a per-division unspecified result

      --r1cs-lc-elim-thresh <LC_ELIM_THRESH>
          linear combination constraints up to this size will be eliminated
          
          [env: R1CS_LC_ELIM_THRESH=]
          [default: 50]

      --field-builtin <BUILTIN>
          Which field to use
          
          [env: FIELD_BUILTIN=]
          [default: bls12381]

          Possible values:
          - bls12381: BLS12-381 scalar field
          - bn254:    BN-254 scalar field

      --field-custom-modulus <CUSTOM_MODULUS>
          Which modulus to use (overrides [FieldOpt::builtin])
          
          [env: FIELD_CUSTOM_MODULUS=]
          [default: ]

      --ir-field-to-bv <FIELD_TO_BV>
          Which field to use
          
          [env: IR_FIELD_TO_BV=]
          [default: wrap]

          Possible values:
          - wrap:  x % 2^b
          - panic: a panic

      --fmt-use-default-field <USE_DEFAULT_FIELD>
          Which field to use
          
          [env: FMT_USE_DEFAULT_FIELD=]
          [default: true]
          [possible values: true, false]

      --fmt-hide-field <HIDE_FIELD>
          Always hide the field
          
          [env: FMT_HIDE_FIELD=]
          [default: false]
          [possible values: true, false]

      --zsharp-isolate-asserts <ISOLATE_ASSERTS>
          In Z#, "isolate" assertions. That is, assertions in if/then/else expressions only take effect if that branch is active.
          
          See `--branch-isolation` in [ZoKrates](https://zokrates.github.io/language/control_flow.html).
          
          [env: ZSHARP_ISOLATE_ASSERTS=]
          [default: false]
          [possible values: true, false]

      --datalog-rec-limit <N>
          How many recursions to allow
          
          [env: DATALOG_REC_LIMIT=]
          [default: 5]

      --datalog-lint-prim-rec <LINT_PRIM_REC>
          Lint recursions that are allegedly primitive recursive
          
          [env: DATALOG_LINT_PRIM_REC=]
          [default: false]
          [possible values: true, false]

      --c-sv-functions
          Enable SV competition builtin functions
          
          [env: C_SV_FUNCTIONS=]

      --c-assert-no-ub
          Assert no undefined behavior
          
          [env: C_ASSERT_NO_UB=]

  -h, --help
          Print help information (use `-h` for a summary)

```
```console
$ parser -h
? 0
Options that configure CirC

Usage: parser [OPTIONS]

Options:
      --r1cs-verified <VERIFIED>
          Use the verified field-blaster [env: R1CS_VERIFIED=] [default: false] [possible values: true, false]
      --r1cs-div-by-zero <DIV_BY_ZERO>
          Which field division-by-zero semantics to encode in R1cs [env: R1CS_DIV_BY_ZERO=] [default: incomplete] [possible values: incomplete, zero, non-det]
      --r1cs-lc-elim-thresh <LC_ELIM_THRESH>
          linear combination constraints up to this size will be eliminated [env: R1CS_LC_ELIM_THRESH=] [default: 50]
      --field-builtin <BUILTIN>
          Which field to use [env: FIELD_BUILTIN=] [default: bls12381] [possible values: bls12381, bn254]
      --field-custom-modulus <CUSTOM_MODULUS>
          Which modulus to use (overrides [FieldOpt::builtin]) [env: FIELD_CUSTOM_MODULUS=] [default: ]
      --ir-field-to-bv <FIELD_TO_BV>
          Which field to use [env: IR_FIELD_TO_BV=] [default: wrap] [possible values: wrap, panic]
      --fmt-use-default-field <USE_DEFAULT_FIELD>
          Which field to use [env: FMT_USE_DEFAULT_FIELD=] [default: true] [possible values: true, false]
      --fmt-hide-field <HIDE_FIELD>
          Always hide the field [env: FMT_HIDE_FIELD=] [default: false] [possible values: true, false]
      --zsharp-isolate-asserts <ISOLATE_ASSERTS>
          In Z#, "isolate" assertions. That is, assertions in if/then/else expressions only take effect if that branch is active [env: ZSHARP_ISOLATE_ASSERTS=] [default: false] [possible values: true, false]
      --datalog-rec-limit <N>
          How many recursions to allow [env: DATALOG_REC_LIMIT=] [default: 5]
      --datalog-lint-prim-rec <LINT_PRIM_REC>
          Lint recursions that are allegedly primitive recursive [env: DATALOG_LINT_PRIM_REC=] [default: false] [possible values: true, false]
      --c-sv-functions
          Enable SV competition builtin functions [env: C_SV_FUNCTIONS=]
      --c-assert-no-ub
          Assert no undefined behavior [env: C_ASSERT_NO_UB=]
  -h, --help
          Print help information (use `--help` for more detail)

```

### Defaults
```console
$ parser
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: false,
            div_by_zero: Incomplete,
            lc_elim_thresh: 50,
        },
        field: FieldOpt {
            builtin: Bls12381,
            custom_modulus: "",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: false,
        },
        datalog: DatalogOpt {
            rec_limit: 5,
            lint_prim_rec: false,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```

### R1CS Options
```console
$ parser --r1cs-verified true
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: true,
            div_by_zero: Incomplete,
            lc_elim_thresh: 50,
        },
        field: FieldOpt {
            builtin: Bls12381,
            custom_modulus: "",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: false,
        },
        datalog: DatalogOpt {
            rec_limit: 5,
            lint_prim_rec: false,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```
```console
$ parser --r1cs-verified false
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: false,
            div_by_zero: Incomplete,
            lc_elim_thresh: 50,
        },
        field: FieldOpt {
            builtin: Bls12381,
            custom_modulus: "",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: false,
        },
        datalog: DatalogOpt {
            rec_limit: 5,
            lint_prim_rec: false,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```
```console
$ parser --r1cs-div-by-zero non-det
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: false,
            div_by_zero: NonDet,
            lc_elim_thresh: 50,
        },
        field: FieldOpt {
            builtin: Bls12381,
            custom_modulus: "",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: false,
        },
        datalog: DatalogOpt {
            rec_limit: 5,
            lint_prim_rec: false,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```
```console
$ parser --r1cs-div-by-zero incomplete
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: false,
            div_by_zero: Incomplete,
            lc_elim_thresh: 50,
        },
        field: FieldOpt {
            builtin: Bls12381,
            custom_modulus: "",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: false,
        },
        datalog: DatalogOpt {
            rec_limit: 5,
            lint_prim_rec: false,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```
```console
$ parser --r1cs-div-by-zero zero
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: false,
            div_by_zero: Zero,
            lc_elim_thresh: 50,
        },
        field: FieldOpt {
            builtin: Bls12381,
            custom_modulus: "",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: false,
        },
        datalog: DatalogOpt {
            rec_limit: 5,
            lint_prim_rec: false,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```
```console
$ R1CS_DIV_BY_ZERO=non-det parser --r1cs-lc-elim-thresh 11
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: false,
            div_by_zero: NonDet,
            lc_elim_thresh: 11,
        },
        field: FieldOpt {
            builtin: Bls12381,
            custom_modulus: "",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: false,
        },
        datalog: DatalogOpt {
            rec_limit: 5,
            lint_prim_rec: false,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```
```console
$ R1CS_VERIFIED=true R1CS_LC_ELIM_THRESH=10 parser
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: true,
            div_by_zero: Incomplete,
            lc_elim_thresh: 10,
        },
        field: FieldOpt {
            builtin: Bls12381,
            custom_modulus: "",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: false,
        },
        datalog: DatalogOpt {
            rec_limit: 5,
            lint_prim_rec: false,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```

### Field Options

```console
$ FIELD_CUSTOM_MODULUS=7 parser --field-builtin bn254
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: false,
            div_by_zero: Incomplete,
            lc_elim_thresh: 50,
        },
        field: FieldOpt {
            builtin: Bn254,
            custom_modulus: "7",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: false,
        },
        datalog: DatalogOpt {
            rec_limit: 5,
            lint_prim_rec: false,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```

```console
$ FIELD_BUILTIN=bn254 parser --field-custom-modulus 7
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: false,
            div_by_zero: Incomplete,
            lc_elim_thresh: 50,
        },
        field: FieldOpt {
            builtin: Bn254,
            custom_modulus: "7",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: false,
        },
        datalog: DatalogOpt {
            rec_limit: 5,
            lint_prim_rec: false,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```

### Z# Options

```console
$ ZSHARP_ISOLATE_ASSERTS=true parser
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: false,
            div_by_zero: Incomplete,
            lc_elim_thresh: 50,
        },
        field: FieldOpt {
            builtin: Bls12381,
            custom_modulus: "",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: true,
        },
        datalog: DatalogOpt {
            rec_limit: 5,
            lint_prim_rec: false,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```

```console
$ parser --zsharp-isolate-asserts true
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: false,
            div_by_zero: Incomplete,
            lc_elim_thresh: 50,
        },
        field: FieldOpt {
            builtin: Bls12381,
            custom_modulus: "",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: true,
        },
        datalog: DatalogOpt {
            rec_limit: 5,
            lint_prim_rec: false,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```

### Datalog Options

```console
$ DATALOG_LINT_PRIM_REC=true parser --datalog-rec-limit 10
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: false,
            div_by_zero: Incomplete,
            lc_elim_thresh: 50,
        },
        field: FieldOpt {
            builtin: Bls12381,
            custom_modulus: "",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: false,
        },
        datalog: DatalogOpt {
            rec_limit: 10,
            lint_prim_rec: true,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```

```console
$ DATALOG_REC_LIMIT=15 parser --datalog-lint-prim-rec true
? 0
BinaryOpt {
    circ: CircOpt {
        r1cs: R1csOpt {
            verified: false,
            div_by_zero: Incomplete,
            lc_elim_thresh: 50,
        },
        field: FieldOpt {
            builtin: Bls12381,
            custom_modulus: "",
        },
        ir: IrOpt {
            field_to_bv: Wrap,
        },
        fmt: FmtOpt {
            use_default_field: true,
            hide_field: false,
        },
        zsharp: ZsharpOpt {
            isolate_asserts: false,
        },
        datalog: DatalogOpt {
            rec_limit: 15,
            lint_prim_rec: true,
        },
        c: COpt {
            sv_functions: false,
            assert_no_ub: false,
        },
    },
}

```
