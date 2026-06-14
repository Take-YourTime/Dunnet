# Demo Commands

Get standard output from original Dunnet

```bash
emacs -batch -l dunnet < testactions > testactions_standard_output.txt

emacs -batch -l dunnet < delimiters > delimiters_standard_output.txt

emacs -batch -l dunnet < heavytest > heavytest_standard_output.txt
```

Get output from self-made Dunnet

```bash
./dunnet < testactions > testactions_my_output.txt

./dunnet < delimiters> delimiters_my_output.txt

./dunnet < heavytest > heavytest_my_output.txt
```

Compare two files to get the difference

```bash
diff -y -w testactions_my_output.txt testactions_standard_output.txt

diff -y -w delimiters_my_output.txt delimiters_standard_output.txt

diff -y -w heavytest_my_output.txt heavytest_standard_output.txt
```