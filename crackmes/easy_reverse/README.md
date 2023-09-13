CTF Download: https://crackmes.one/crackme/5b8a37a433c5d45fc286ad83

# Writeup
This is my first CTF which I found by watching this [video](https://www.youtube.com/watch?v=fTGTnrgjuGA).

I started off by running the crackme. After running it, I saw it is expecting a password.

In Ghidra I created a new project then imported the crackme, then I analysed the decompiled code and "fixed" it.
My first step was to fix the main method declaration, by default it looked like this: \
![defaultSignature](https://github.com/XiovV/reverse_engineering/assets/20326947/27f417a4-8ab2-4bd6-b4f4-6b0a72bc4b05) \
Then I right clicked it and selected Edit Function Signature. I changed the return type to int, and renamed the arguments following the C++ standard. First argument is int argc, the second one is char **argv ([explanation](https://en.cppreference.com/w/cpp/language/main_function)).

This also helped with making the decompiled code look a bit cleaner. This is what it looked like before:
![defaultCode](https://github.com/XiovV/reverse_engineering/assets/20326947/da1bded4-d06a-4462-971d-f6f3085cf8ca)


This is what it looks like after fixing the main function signature. We can see that the argv array is now properly displayed: \
![image](https://github.com/XiovV/reverse_engineering/assets/20326947/5480c1fd-a032-46d9-85ea-1cb9d1786616)

In order to make the code a bit easier to understand I renamed the `sVar1` variable to `passwordInputLength`. If we look at line 8, we see that `sVar1` is set to `strlen(argv[1]`, hence the new name.

# Solution
On line 10 we can see an if statement, where the program checks if the element at the 4th index of our password input is "@". This, combined with the fact that this only executed if the input is 10 characters long, we know that the password has to be exactly 10 characters long, and the 5th character should be "@".
Which means that this password should work: 1234@67890. Let's give it a shot: \
![image](https://github.com/XiovV/reverse_engineering/assets/20326947/fa1b2e88-7586-4000-b8c2-15b71532f4f2)
And sure enough, it works!
