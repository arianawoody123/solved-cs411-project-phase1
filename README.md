Download Link: https://assignmentchef.com/product/solved-cs411-project-phase1
<br>
You are required to develop a simplified version of the TextSecure protocol, which provides forward secrecy and deniability. Working in the project will provide you with insight for a practical cryptographic protocol, a variant of which is used in different applications such as WhatsApp.

<h1>1      Introduction</h1>

The project has three phases:

<ul>

 <li><strong>Phase I </strong>Developing software for the Registration and the Station-to-Station (STS) protocols. All coding development will be in Python programming language.</li>

 <li>TBA</li>

 <li>TBA</li>

</ul>

More information about Phase I is given in the subsequent section.

<h1>2 Phase I: Developing software for the Registration and Stationto-Station Protocols</h1>

In this phase of the project, you are required to upload one file: “Client.py”. You will be provided with “Client basics.py”, which includes all required communication codes.

In the Registration protocol (see below) you will register your long term public key with a server. You will also simulate the STS protocol with the same server. The server is accessible via cryptlygos.pythonanywhere.com. You may find the connection details in “Client basics.py”. JSON format should be used in all communication steps.

In the Station-to-Station (STS) protocol, each party must have a long term public and private key pair to sign messages and verify signatures. A variant of Elliptic Curve Digital Signature Algorithm (ECDSA) will be used with NIST-256 curve in this project. See Section 2.3 for the description of the variant of the ECDSA algorithm that will be used in the project. You should select “secp256k1” for the elliptic curve in your python code. <strong>Note that you will NOT use the ECDSA algorithm in the slides.</strong>

<h2>2.1     Registration</h2>

The long term public key of the server <em>Q<sub>SL </sub></em>is given below.

X:0xc1bc6c9063b6985fe4b93be9b8f9d9149c353ae83c34a434ac91c85f61ddd1e9

Y:0x931bd623cf52ee6009ed3f50f6b4f92c564431306d284be7e97af8e443e69a8c

In this part, firstly you are required to generate a long-term private and public key pair <em>s<sub>L </sub></em>and <em>Q<sub>L </sub></em>for yourself. The key generation is described in “Key generation” algorithm in Section 2.3. Then, you are required to register with the server. The registration operation consists of four steps:

<ol>

 <li>After you generate your long-term key pair, you should sign your ID (e.g. 18007). The details of the signature scheme is given in the signature generation algorithm in Section 2.3. Then, you will send a message, which contains your student ID, the signature tuple and your long-term public key, to the server. The message format is</li>

</ol>

{‘ID’: stuID, ‘H’: h, ‘S’: s, ‘LKEY.X’: lkey.x, ‘LKEY.Y’: lkey.y}

where stuID is your student ID, h and s are signature tuple and lkey.x and lkey.y are the <em>x </em>and <em>y </em>and coordinates of your long-term public key, respectively. A sample message is given in ‘samples.txt’.

<ol start="2">

 <li>If your message is verified by the server successfully, you will receive an e-mail, which includes your ID, your public key and a verification code: code.</li>

 <li>If your public key is correct in the verification e-mail, you will send another message to the server to authenticate yourself. The message format is “{‘ID’: stuID, ‘CODE’: code}”, where code is verification code which, you have received in the previous step. A sample message is given below.</li>

</ol>

{‘ID’: 18007, ‘CODE’: 209682}

<ol start="4">

 <li>If you send the correct verification code, you will receive an acknowledgement message via e-mail, which states that you are registered with the server successfully.</li>

</ol>

Once you register with the server successfully, you are not required to perform registration step again as the server stores your long-term public key to identify you. <strong>You need to store your long-term key pair as you will use them until the end of the project.</strong>

<h2>2.2       Station-to-Station Protocol</h2>

Here, you will develop a python code to implement the STS protocol. For the protocol, you will need the elliptic curve digital signature algorithm described in Section 2.3. The protocol has seven steps as explained below.

<ol>

 <li>You are required to generate an ephemeral key pair <em>s<sub>A </sub></em>and <em>Q<sub>A</sub></em>, which denote private and public keys, respectively. The key generation is described in “Key generation” algorithm in Section 2.3</li>

</ol>

Then, you will send a message, which includes your student ID and the ephemeral public key, to the server. The message format is “{‘ID’: stuID, ‘EKEY.X’: ekey.x, ‘EKEY.Y’: ekey.y”, where ekey.x and ekey.y are the <em>x </em>the <em>y </em>coordinates of your ephemeral public key, respectively. A sample message is given in ‘samples.txt’.

<ol start="2">

 <li>After you send your ephemeral public key, you will receive the ephemeral public key <em>Q<sub>B </sub></em>of the server. The message format is “{‘SKEY.X’: skey.x, ‘SKEY.Y: skey.y}”, where x and skey.y denote the <em>x </em>and <em>y </em>coordinates of <em>Q<sub>B</sub></em>, respectively. A sample message is given in ‘samples.txt’.</li>

 <li>After you receive <em>Q<sub>B</sub></em>, you are required to compute the session key <em>K </em>as follows.

  <ul>

   <li><em>T </em>= <em>s<sub>A</sub>Q<sub>B</sub></em></li>

   <li><em>U </em>= {<em>x</em>||<em>T.y</em>||“<em>BeY ourselfNoMatterWhatTheySay</em>”}, where <em>T.x </em>and <em>T.y </em>denote the <em>x </em>and <em>y </em>coordinates of <em>T</em>, respectively.</li>

   <li><em>K </em>= SHA3 256(<em>U</em>)</li>

  </ul></li>

</ol>

A sample for this step is provided in ‘samples.txt’.

<ol start="4">

 <li>After you compute <em>K</em>, you should create a message <em>W</em><sub>1</sub>, which includes your and the server’s ephemeral public keys, generate a signature <em>Sig<sub>A </sub></em>using <em>s<sub>L </sub></em>for the message <em>W</em><sub>1</sub>. After that, you should encrypt the signature using AES in the Counter Mode (AES-CTR). The required operations are listed below.

  <ul>

   <li><em>W</em><sub>1 </sub>= <em>Q<sub>A</sub>.x</em>||<em>Q<sub>A</sub>.y</em>||<em>Q<sub>B</sub>.x</em>||<em>Q<sub>B</sub>.y</em>, where <em>Q<sub>A</sub>.x</em>, <em>Q<sub>A</sub>.y</em>, <em>Q<sub>B</sub>.x </em>and <em>Q<sub>B</sub>.y </em>are the <em>x </em>and <em>y </em>coordinates of <em>Q<sub>A </sub></em>and <em>Q<sub>B</sub></em>, respectively.</li>

   <li>(<em>Sig<sub>A</sub>.s,Sig<sub>A</sub>.h</em>) = Sign<em><sub>s</sub></em><em><sub>L</sub></em>(<em>W</em><sub>1</sub>)</li>

   <li><em>Y</em><sub>1 </sub>= <em>E<sub>K</sub></em>(“<em>s</em>”||<em>Sig<sub>A</sub>.s</em>||“<em>h</em>”||<em>Sig<sub>A</sub>.h</em>)</li>

  </ul></li>

</ol>

Then, you should concatenate the 8-byte nonce Nonce<em><sub>L </sub></em>and <em>Y</em><sub>1 </sub>and send {Nonce<em><sub>L</sub></em>||<em>Y</em><sub>1</sub>} to the server. Note that, you should convert the ciphertext from byte array to integer. A sample for this step is provided in ‘samples.txt’.

<ol start="5">

 <li>If your signature is valid, the server will perform the same operations which are explained in step 4. It creates a message <em>W</em><sub>2</sub>, which includes the server’s and your ephemeral public keys, generate a signature <em>Sig<sub>B </sub></em>using <em>s<sub>SL</sub></em>, where <em>s<sub>SL </sub></em>is the long-term private key of the server. After that, it will encrypt the signature using AES-CTR.

  <ul>

   <li><em>W</em><sub>2 </sub>= <em>Q<sub>B</sub>.x</em>||<em>Q<sub>B</sub>.y</em>||<em>Q<sub>A</sub>.x</em>||<em>Q<sub>A</sub>.y</em>. (<strong>Note that, </strong><em>W</em><sub>1 </sub><strong>and </strong><em>W</em><sub>2 </sub><strong>are different</strong>.)</li>

   <li>(<em>Sig<sub>B</sub>.s,Sig<sub>B</sub>.h</em>) = Sign<em><sub>s</sub></em><em><sub>L</sub></em>(<em>W</em><sub>1</sub>)</li>

   <li><em>Y</em><sub>2 </sub>= <em>E<sub>K</sub></em>(“<em>s</em>”||<em>Sig<sub>B</sub>.s</em>||“<em>h</em>”||<em>Sig<sub>B</sub>.h</em>)</li>

  </ul></li>

</ol>

Finally, it will concatenate the 8-byte nonce Nonce<em><sub>SL </sub></em>to <em>Y</em><sub>2 </sub>and send {Nonce<em><sub>SL</sub></em>||<em>Y</em><sub>2</sub>} to you. After you receive the message, you should decrypt it and verify the signature. The signature verification algorithm is explained in Section 2.3. A sample for this step is provided in ‘samples.txt’.

<ol start="6">

 <li>Then, the server will send to you another message <em>E<sub>K</sub></em>(<em>W</em><sub>3</sub>) <a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> where, <em>W</em><sub>3 </sub>= {Rand||Message}. Here, Rand and Message denote an 8-byte random number and a meaningful message, respectively. You need to decrypt the message, and obtain the meaningful message and the random number Rand. A sample for this step is provided in ‘samples.txt’.</li>

 <li>Finally, you will prepare a message <em>W</em><sub>4 </sub>= {(Rand+1)||Message} and send <em>E<sub>K</sub></em>(<em>W</em><sub>4</sub>) <a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a> to the server. Sample messages for this step is given below.</li>

</ol>

<em>W</em><sub>4 </sub>: 86987

<em>MessagetoServer </em>: 86987

<ol start="8">

 <li>If your message is valid, the server will send a response message as <em>E<sub>K</sub></em>(“<em>SUCCESSFUL</em>”||Rand + 2) <sup>1</sup></li>

</ol>

<h2>2.3           Elliptic Curve Digital Signature Algorithm (ECDSA)</h2>

Here, you will develop a Python code that includes functions for signing given any message and verifying the signature. For ECDSA, you will use an algorithm, which consists of three functions as follows:

<ul>

 <li><strong>Key generation: </strong>A user picks a random secret key 0 <em>&lt; s<sub>A </sub>&lt; q </em>− 1 and computes the public key <em>Q<sub>A </sub></em>= <em>s<sub>A</sub>P</em>.</li>

 <li><strong>Signature generation: </strong>Let <em>m </em>be an arbitrary length message. The signature is computed as follows:

  <ol>

   <li><em>k </em>← Z<em>n</em>, (i.e., <em>k </em>is a random integer in [1<em>,n </em>− 2]).</li>

   <li><em>R </em>= <em>k </em> <em>P</em></li>

   <li><em>r </em>= <em>x </em>(mod <em>n</em>), where <em>R.x </em>is the <em>x </em>coordinate of <em>R</em></li>

   <li><em>h </em>= SHA3 256(<em>m </em>+ <em>r</em>) (mod <em>n</em>)</li>

   <li><em>s </em>= (<em>s<sub>A </sub></em> <em>h </em>+ <em>k</em>) (mod <em>n</em>)</li>

   <li>The signature for <em>m </em>is the tuple (<em>h,s</em>).</li>

  </ol></li>

 <li><strong>Signature verification: </strong>Let <em>m </em>be a message and the tuple (<em>s,h</em>) is a signature for <em>m</em>. The verification proceeds as follows:

  <ul>

   <li><em>V </em>= <em>sP </em>− <em>hQ<sub>A</sub></em></li>

   <li><em>v </em>= <em>x </em>(mod <em>n</em>), where <em>V.x </em>is x coordinate of <em>V</em></li>

   <li><em>h</em><sup>0 </sup>= SHA3 256(<em>m </em>+ <em>v</em>) (mod <em>n</em>)</li>

   <li>Accept the signature only if <em>h </em>= <em>h</em><sup>0 </sup><strong>– </strong>Reject it otherwise.</li>

  </ul></li>

</ul>

Note that the signature generation and verification of this ECDSA are different from the one discussed in the lecture.

<a href="#_ftnref1" name="_ftn1">[1]</a> The first 8-byte of message, which you receive in this step, is nonce.

<a href="#_ftnref2" name="_ftn2">[2]</a> All encrypted messages must be generated using unique nonce values. For each encryption, you must use AES.new(). Then, you must concatenate nonce and ciphertext.