# Flare-on challenge 1 Fidler
[Compete in Flare-on](https://2020.flare-on.com/challenges)

## Initial inspection
After opening the application I was immediately greeted by a password screen. Luckily enough the source code was packed so we can try and find the password.

## Getting the password
Opening fidler.py immediately greets us with a function

```python
def password_check(input):
    altered_key = 'hiptu'
    key = ''.join([chr(ord(x) - 1) for x in altered_key])
    return input == key
```

Running this code and then printing the key variable shows us that the password is 'ghost', inputting this in the password screen shows another screen.
This screen shows a cookie clicker with the objective to get 100 billion coins.

## Making money
You could click this kitty 100 billion times or even buy some autoclickers but we don't have time for that. Instead we can run the source code

```sh
python fidler.py
```

At first I got an error that pygame wasn't installed but we can just install pygame using pip to fix this, now we have a functioning version of the game and we can edit it's source code. The next step was finding where the amount of coins is stored or where you gain cookies. Some quick looking in the code found the following function:

```python
def cat_clicked():
    global current_coins
    current_coins += 1
    return
```

Ok, let's update that function to give ourselves more coins

```python
def cat_clicked():
    global current_coins
    current_coins += 10000000000
    return
```

And that's it, we can now submit the flag.