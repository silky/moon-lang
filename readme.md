## Moon: an universal code-interchange format

Moon is an universal code-interchange format. It is designed to transmit code through the internet, and to run it safely and efficiently. It is:

- **Portable:** the entire language (parser, optimizer and compiler) is just a [5kb JS file](https://github.com/MaiaVictor/moon/blob/master/moon-lang/moon-core.js) (gzipped).

- **Fast:** Moon is highly optimized and can run [5-100x faster](moon-demo/demo-performance.js) than popular FP libs (Ramda, Lodash, Undersore, etc.).

- **Safe:** since Moon is 100% pure and side-effect free, you can safely run code from untrusted sources.

- **Lightweight:** thanks to a [compact binary format](https://tromp.github.io/cl/LC.pdf), Moon bundles are much smaller than JS bundles.

- **Decentralized:** you can publish and import code from [Ethereum](https://www.ethereum.org), a censure-resistant computer network.

In essence, Moon is a minimal high-level subset of JS which has just enough stuff to run fastly and safely. Think of it as JSON for algorithms.

## Usage

1. **Inside JavaScript:**

    Install:

    ```bash
    npm install moon-lang
    ```

    And then require it with:

    ```javascript
    const M = require("moon-lang");

    // User-submitted algorithms
    const userAlgos = M.parse(`{
      "successor": x. (add x 1),
      "square": x. (mul x x),
      "factorial": x. (for 1 (add 1 x) 1 (mul))
    }`);

    // Since they were parsed by Moon, you can run them safely!
    console.log(userAlgos.factorial(4)); // output: 24
    ```


2. **Command line:**

    Make sure you're using Node.js 8.0+:

    ```bash
    node --version
    ```

    If not, install it:

    ```bash
    nvm install 8.1.3
    nvm use 8.1.3
    ```

    Install `moon-tool` globally:

    ```bash
    npm install -g moon-tool
    ```

    Then just use it:

    ```bash
    # creates a hello world file
    echo '"Hello World!"' >> hello.moon

    # prints a hello world!
    moon run hello

    # prints two hello worlds!
    moon run "[hello, hello]"

    # prints many hellos!
    moon run "(for 0 8 hello i.h.(con h h))"

    # shows the Ethereum address of this Moon CLI
    moon address

    # checks its balance (must have a few USD cents to publish)
    moon balance

    # publishes your hello world to Ethereum/Swarm
    moon publish hello

    # removes it from the local system - oh no, destroyed information!
    rm hello.moon

    # not really, it is on Ethereum/Swarm now
    moon run "[hello, hello]"

    # everyone can directly reference your 'hello' on its code now!
    ```

## Articles

- [Importing and publishing Moon code from Ethereum/Swarm](moon-gist/importing-and-publishing-moon-code-from-ethereum-swarm.md)

## Demos

- [Language overview ("learn Moon in Y minutes")](moon-demo/demo-language-overview.js)

- [Optimizing functional JavaScript code by 5-100x](moon-demo/demo-performance.js)

- [Tail calls and pure fors](moon-demo/demo-tail-calls-vs-pure-fors.js)

- [Lightweight monadic notation as a generalization of async/await](moon-demo/demo-monadic-notation.js)

- [Custom importers](moon-demo/demo-importers.md)

## Soon

- Publish Ethereum DApps with `moon pub myDapp.moon`

- Use [ENS](https://ens.domains) on Moon's contract for proper namespacing

- A Moon DApp

- System-F based type inferencer / checker?

- Better syntax?

## Disclaimers

1. Moon is in experimental stage, wasn't audited and **certainly** has nasty bugs. Don't use it in production yet.

2. Sorry for creating a new programming language.

3. [be kind]
