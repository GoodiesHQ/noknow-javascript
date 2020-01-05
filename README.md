<h1 align="center">NoKnow</h1>
<div align="center">
  <strong>Zero-Knowledge Proof implementation in pure javascript</strong>
</div>
<br />
<div align="center">
  <img src="http://badges.github.io/stability-badges/dist/experimental.svg" alt="Experimental" />
</div>
<div align="center">
  <sub>
    Built with ❤︎ by <a href="https://www.linkedin.com/in/austinarcher/">Austin Archer</a> :)
  </sub>
</div>
<br />



## Table of Contents
- [Credits](#credits)
- [Purpose](#purpose)
- [How it Works](#how-it-works)
- [API](#api)
- [Example Usage](#example-usage)


## Credits
I am far from a mathematician and did not design the protocol myself. The proofs used are rather complex in nature. I will do my best to explain its functionality, but please refer to the research papers on which this implementation is based as it does a far more complete job with explanation than I. **Note:** the paper's implementation uses the set of all real numbers as the cyclic group for simplicity. In this implementation, the cyclic group used is an elliptic curve.

[Implementing Zero-Knowledge Authentication with Zero Knowledge](https://ojs.pythonpapers.org/index.php/tppm/article/view/155) by Brandon Lum Jia Jun


## Purpose
Zero-Knowledge Proofs are undoubtedly the future of authentication security within various IT and application development industrires. The ability to verify the veracity of a claim (ex: proving that you know a secret password), without divulging any information about the claim itself (ex: passwords or hashes), allows for servers to guarantee secure AAA operations (authentication, authorization, and accounting) without exposing private information. `NoKnow` is an implementation of a [Non-Interactive Zero-Knowledge Proof](https://en.wikipedia.org/wiki/Non-interactive_zero-knowledge_proof) protocol specifically designed for verifying text-based secrets, which is ideal for passwords or other authentication means.


## How It Works
TODO: explain how it works

## API

The `noknow` Javascript API is meant to be simple and intuitive:

### Core Components

#### noknow.core.ZKParameters:
The parameters used to initialize the Elliptic Curve within the Zero-Knowledge crypto system.

    class ZKParameters
    /**
     * @param {string} curve - the name of the standardized elliptic curve to use for cyclic group point generation
     * @param {BigInt} d - the large prime number used to generate an elliptic curve point and as a modulo
     */
        
#### noknow.core.ZKSignature:
A cryptographic public signature created from an Elliptic Curve and a user-provided secret (ex: a password) which should be stored by the server (along with the `ZKParameters` used) and can be used to validate future challenges.


    class ZKSignature
    /**
     * @param {ZKParameters} params - The ZKParemeters used by the algorithm
     * @param {BigInt} signature - the calculated signature derived from the ZK curve and user secret
     */


#### noknow.core.ZKChallenge:

    class ZKChallenge
    /**
     * @param {ZKParameters} params - The ZKParameters used by the algorithm
     * @param {BigInt} token - Randomly generated server-provided token
     * @param {BigInt} c - A hash derived from the signature, a random point, and random token
     * @param {BigInt} z - A value derived from the random point and password hash
     */        
        
### Proof
 
The `ZKProof` class is the central component of `NoKnow` and its state (defined by `ZKParameters`) should be inherently known to both the Client (Prover) and Server (Verifier).

#### instance methods
<table>
  <tr>
    <th>Method</th>
    <th><img width=600 />Parameters</th>
    <th>Role</th>
    <th>Purpose</th>
  </tr>
  <tr>
    <td><code>create_signature</code></td>
    <td><code>secret: Union[str, bytes]</code></td>
    <td>Prover</td>
    <td>Create a cryptographic signature derived from the value <code>secret</code> to be generated during initial registration and stored for subsequent challenge proofs</td>
  </tr>
  <tr>
    <td><code>create_challenge</code></td>
    <td><code>secret: Union[str, bytes]</code> <br /> <code>token: int</code></td>
    <td>Prover</td>
    <td>Create a ZK challenge with the <code>secret</code> a given randomly generated <code>token</code> provided by the server</td>
  </tr>
  <tr>
    <td><code>prove_challenge</code></td>
    <td><code>challenge: ZKChallenge</code> <br /> <code>signature: ZKSignature</code> <br /> <code>token: int</code></td>
    <td>Verifier</td>
    <td>Verify the user-provided <code>challenge</code> against the stored <code>signature</code> and randomly generated <code>token</code> to verify the validity of the challenge</td>
  </tr>
</table>


## Example Usage
TODO: Include example usage

#### Example 1
    const  noknow = require("noknow");

    async function main(){
      try{
        let zk = await noknow.ZKProof.new()
        let signature = zk.create_signature("Passw0rd1")
        let token = noknow.ZKProof.random_token()
        let challenge = zk.create_challenge("Passw0rd1", token)
        console.log("Proving Challenge...", zk.prove_challenge(challenge, signature, token) ? "Success!" : "Failure!")
      } catch(err) {
        console.log("Error:", err.message)
      }
    }

    main()

