## Programming languages by lines of code

2.52M erlang
1.67M golang
1.29M python
1.18M perl
1.01M racket
838K pharo
827K rust
810K s7
552K ghc
400K groovy
385K ocaml
375K guile
369K gnu-smalltalk
348K scala
345K objectscript
299K micropython
278K nim
271K tcl
276K haxe
258K duktape
207K gawk
141K angelscript
126K cuis smalltalk
90.9K chibi scheme
88.7K luajit
83.6K rakudo
82.9K io
74.2K mruby
73K clojure
68.7K picoc
68K wren
65.2K dao
62.8K chicken scheme
52.6K tcc
34.8K luaj
32K lua
32K myrddin
27.1K neko
26.9K chaiscript
20K gravity
17.6K squirrel
5.7K lil
5.5K tinyscheme
2.34K minischem
1.2K minilisp
502 c4

## Programming languages by time

fizzbuzz implementation from RosettaCode 100 times:

```terminal
time sh -c 'for i in $(seq 1 100); do tinyscheme fizz.scm; done
```

0.151 tinyscheme
0.145 awk
0.086 tcc

## Programming languages sizes

Small is < 3MB on top of `libc6`

### By family

#### Schemes
748.2M	racket_total
167.8M	chicken-bin_total (93.5)
96.2M	gambc_total (21.9)
78.8M	chezscheme:amd64_total (4.5M)
<mark>74.5M	tinyscheme_total (0.2M)</mark>

####  Lisps
119.8M	sbcl_total (45.5M)
119.1M	cmucl:i386_total (44.8)
109.3M	clisp_total (35M)

#### Shells
93.3M	zsh_total (19.0M)
87.7M	bash_total (13.4M)
<mark>76.0M	mksh_total (1.7M)</mark>
<mark>75.8M	rc_total (1.5M)</mark>
<mark>75.1M	ash_total (0.8M)</mark>
<mark>74.8M	dash_total (0.5M)</mark>

#### Others
733.2M	clang_total (658.9M)
471.0M	gccgo_total (396.7M)
298.7M	haxe_total (224.4M)
277.2M	gcc_total (202.9M)
152.9M	python3_total (78.6M)
146.6M	tcc_total (72.3M)
88.6M	gnu-smalltalk_total (14.3M)
81.9M	tcl_total (7.6M)
79.1M	gawk_total (4.8M)
77.6M	gforth_total (3.3M)
<mark>76.1M	lua5.4_total (1.8M)</mark>
<mark>75.6M	luajit_total (1.3M)</mark>
<mark>75.1M	busybox_awk_total (0.8M)</mark>

### By size

#### x1 (<0.29M)
<mark>74.5M	tinyscheme_total (0.2M)</mark>

#### x2 (< 0.58M)
<mark>74.8M	dash_total (0.5M)</mark>

#### x4 (< 1.2M)
75.1M	busybox_awk_total (0.8M)

#### x8 (< 2.34M)
<mark>75.6M	luajit_total (1.3M)</mark>
76.1M	lua5.4_total (1.8M)

#### x16 (< 4.7M)
77.6M	gforth_total (3.3M)
<mark>78.8M	chezscheme:amd64_total (4.5M)</mark>

#### x32 (< 9.3M)
<mark>79.1M	gawk_total (4.8M)</mark>
81.9M	tcl_total (7.6M)
82.2M	scheme48_total (7.9M)
90.3M	scheme9_total (16.0M)

#### x64 (< 19M)
88.6M	gnu-smalltalk_total (14.3M)

#### x128 (< 37M)
86.2M	scm_total (11.9M)
96.2M	gambc_total (21.9M)
103.7M	gauche_total (29.4M)
<mark>109.3M	cisp_total (35M)</mark>

#### x256 (< 75M) 
119.1M	cmucl:i386_total (44.8)
119.6M	guile-2.2_total (45.3M)
119.8M	sbcl_total (45.5M)
127.3M	guile-3.0_total (53.0M)
<mark>146.6M	tcc_total (72.3M)</mark>
153.6M	nodejs_total (79.3M)

----

### By Alpine container size

#### < 4MB (x1)
<mark>5.33MB busybox ash + awk (0MB)</mark>
8.45MB mawk (3.12MB)
<mark>8.47MB tinyscheme (3.14MB)</mark>
<mark>8.82MB tcc (3.49MB)</mark>
<mark>9.02MB gawk (3.69MB)</mark>

#### < 12MB (x3)
9.47MB luajit (4.14MB)
9.64MB lua5.4 (4.31MB)
10.9MB gforth (5.57MB)
14MB tcl (8.67MB)

#### < 36MB (x9)
9.12MB retroforth (3.79MB)
22.5MB ruby (17.17MB)     
34.3MB vim (28.97MB)
34.6MB chibi-scheme (29.27MB)
34.9MB racket (29.57MB)
54.2MB python3 (48.87MB)

#### < 108MB (x27)
48.4MB sbcl (43.07MB)
50.9MB nodejs (45.57MB)
63.4MB guile (58.07MB)
70.8MB hy (65.47MB)
109MB gcc (103.67MB)

#### < 324MB (x81)
138MB chicken (132.67MB)
223MB clojure (217.67MB)
228MB clang (222.67MB)

#### < 972MB (x243)
386MB go (380.67MB)
817MB rust (811.67MB)