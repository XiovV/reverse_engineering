CTF download: https://crackmes.one/crackme/5c11dcaf33c5d41e58e00578

# Writeup

## Using Ghidra
After importing the binary into Ghidra, we can see that on line 17 `strcmp` is called, which determines whether the input we provided is correct, which means that this is probably where the password is. But before doing anything else, let's fix the code:\
![image](https://github.com/XiovV/reverse_engineering/assets/20326947/c6ce1e71-4c21-4db9-a7a3-133a1f07d5b2)

That looks a little cleaner, but what about the local_1c and local_14 variables? Well, they are holding the solution to this crackme (we know this because local_1c is used in `strcmp`). If we hover over local_1c, we will see the char[] value which is the reverse of "easycrac", I have no idea why it's in reverse, but now that we know it's a char array with a length of 8, we can retype the variable to `char[8]`:\
![image](https://github.com/XiovV/reverse_engineering/assets/20326947/f9be75a3-1693-4755-9244-f91c3f4ebf22)

Now that looks a little better, but our `local_14` variable has disappeared. I have no idea why. However, if we hovered over it before retyping `local_1c`, we would see that it actually holds the value "k". Knowing that, we can safely assume that the password is `easycrack`. Which it is.

I tried using `strings` in order to investigate this weird char array thing, and this is what I got:\
![image](https://github.com/XiovV/reverse_engineering/assets/20326947/b540fd43-aaba-456c-b635-2822ed86f425)
Instead of `easycrack` it says `easycracH`, no idea why, but if I figure it out I will explain it here. There is definitely something strange happening with this string. However, in the ltrace example below it is printed out perfectly.

## Using ltrace
Using ltrace is the easiest method I found for solving this crackme. It displays the password in plaintext:\
![image](https://github.com/XiovV/reverse_engineering/assets/20326947/35b0e01a-792f-4ddc-aa17-f32e800e8cb2)
