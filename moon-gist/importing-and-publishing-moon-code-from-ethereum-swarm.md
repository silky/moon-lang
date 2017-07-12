## Importing and publishing Moon code from Ethereum/Swarm

I just published the first version of [Moon](https://github.com/maiavictor/moon), an universal code-interchange format and pure programming language. I've still not written a proper introduction to it, but, before I do, I'll talk a little bit about one of its coolest features: decentralized imports. Moon doesn't have a "package manager" like most languages, and Moon apps aren't meant to be hosted in normal servers. Instead, whenever you want to publish a library, program or application, the Ethereum/Swarm network is used. Here is how it works:

1. You create a cool program and name it `myLittleProgram.moon`.

2. You go to the terminal and type `moon publish myLittleProgram`.

3. The source code for `myLittleProgram` is sent to a decentralized storage network called Swarm. That network will then store it under an address (a 256-bit string).

4. That address is then given a short name by registering it on Moon's contract on the Ethereum network, following the mutually agreed naming convention.

    * Currently, that convention is just "don't overwrite someone else's name", but it will improve soon.

5. Moon's CLI pays a small fee in Ether (a few USD cents) for the miners of the Ethereum network.

6. Your friend Alice attempts to use `myLittleProgram` inside their Moon code, from another computer.

7. Since Alice doesn't have your program locally, Moon looks up on Ethereum for a hash named `myLittleProgram`.

8. If such hash is found, it is used to download the definition of `myLittleProgram` from Swarm.

9. `myLittleProgram` is imported on Alice's program.

In other words, by merely typing `moon publish myLittleProgram` on your terminal, you allow anyone else in the world to use `myLittleProgram` inside their own programs. If `myLittleProgram` is, for example, an web-application, people will be immediately able to access it from decentralized browsers such as Mist. Since everything is decentralized, you do not need to worry about hosting costs, DDOS attacks, censure and so on. As long as there are people using `myLittleProgram`, it will be kept alive on the network!

Let's see an example. Suppose you've written a Moon program that computes someone's [Body Mass Index](https://en.wikipedia.org/wiki/Body_mass_index) (BMI). You name it `personBMI.moon`:

```javascript
// personBMI.moon
// Computes a person's body-mass-index
person.
  weight: (get person "weight") // in kilograms
  height: (get person "height") // in meters
  (div weight (mul height height))
```

Now you're very proud of your creation and want the world to see it, but you know nothing about hosting. No worries, Moon is here for you. But, first, you need to make sure your CLI has Ether on it:

```bash
$ moon balance
0.07 ETH
```

If your balance is zero, don't worry. Publishing is very cheap (about $0.30 currently - quite a deal for eternity!), so you can just ask a friend to send a few Ethers to your CLI's address:

```bash
$ moon address
0x48ec2F135488C0E0F145e7689783FaE7e305a9ba
```

Once you have it, you can publish your code with another command:

```bash
$ moon publish personBMI
```

You'll see a few hacky messages detailing the process:

```bash
Publishing personBMI.
- Publishing `personBMI` to Swarm...
- Done! SwarmHash: bc338bc430b923caf71c8c2680a4cce23d2d7b3d045f26159df6b47a3daa3194
- Publishing SwarmHash to Ethereum...
- Published! TxHash:  0x835019f12c61a8a3c4d5bb55024adda865f7bcc2a5a06170fa0e622ce8b134eb
- Waiting transaction confirmation (may take about 1 min)...
-- Not Yet. Waiting...
- Block mined!
- Now everyone can use your term, under the name: `personBMI`.
```

And done! Now, anyone else in the world can use `personBMI` by either referencing it inside a Moon program, or by calling it directly on the terminal:

```bash
$ moon run '(personBMI {"weight": 80, "height": 1.8})'
```

Now, suppose a friend has just called him to ask if he is fat. You could be downright honest about it... or you could write a DApp for that!

```javascript
(cli |
  (print "What is your height, in meters?")>
  height: <getLine

  (print "What is your weight, in kilograms?")>
  weight: <getLine

  person: {
    "height": (stn height)
    "weight": (stn weight)
  }

  bmi: (personBMI person)

  isFat: (gtn bmi 25)

  (print (if isFat "Yes." "No."))>

  exit)
```

Save it as `amIFat.moon` and type `moon publish amIFat`. Now, you can just tell him to ask Moon directly:

```bash
    $ moon runIO amIFat
```

Try it!

Note: if you're wondering about the fact we're using `print`s and `getLine`s in a self-proclaimed pure language, don't: that is just an adaptation of [Idris's bang-notation](http://docs.idris-lang.org/en/latest/tutorial/syntax.html) for monadic computations. But that's subject for another post. Check out [Moon's repository](https
