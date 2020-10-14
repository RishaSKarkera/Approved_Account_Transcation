## Authors
  1. Pratheeksha A M (4nm17cs130)
  2. Risha S Karkera (4nm17cs146)
  
## Introduction

Solidity is an object-oriented programming language for writing smart contracts.It is used for implementing smart contracts on various blockchain platforms, most notably, Ethereum. The Approved Account Transaction contract implements the simplest form of a cryptocurrency. The contract allows only its creator to create new coins (different issuance schemes are possible). Anyone can send coins to each other without a need for registering with a username and password, all you need is an Ethereum keypair.<br><br>
      A blockchain is a globally shared, transactional database. This means that everyone can read entries in the database just by participating in the network. If you want to change something in the database, you have to create a so-called transaction which has to be accepted by all others. The word transaction implies that the change you want to make (assume you want to change two values at the same time) is either not done at all or completely applied. Furthermore, while your transaction is being applied to the database, no other transaction can alter it.<br><br>
      If a transfer from one account to another is requested, the transactional nature of the database ensures that if the amount is subtracted from one account, it is always added to the other account. If due to whatever reason, adding the amount to the target account is not possible, the source account is also not modified. Furthermore, a transaction is always cryptographically signed by the sender (creator). This makes it straightforward to guard access to specific modifications of the database.A simple check ensures that only the person holding the keys to the account can transfer money from it.<br>
      
## Program
```
pragma solidity ^0.5.0;

contract ApprovedAT {

    string public Creatorname="Name unknown";
    address public Creator;
    mapping (address => uint) public balances;


    event Sent(address from, address to, uint amount);

    constructor() public {
        Creator = msg.sender;
    }

    function Create(address receiver,string memory Myname) public payable{
        require(msg.sender == Creator);
        balances[receiver] += msg.value;
        Creatorname=Myname;
    }
    
    function CreatorName() public view returns(string memory){
        return Creatorname;
     }
     

    function send(address receiver) public payable {
        require(msg.value <= balances[msg.sender], "Insufficient balance.");
        balances[msg.sender] -= msg.value;
        balances[receiver] += msg.value;
        emit Sent(msg.sender, receiver, msg.value);
    }
    function balance(address _account) external view returns (uint) {
    return balances[_account];
}

    
}
```
## Implementation

We have implemented a Ethereum Smart Contract using Solidity to create a Approved Account Transaction.<br>The line address public Creator; declares a state variable of type address that is publicly accessible. The next line, mapping (address => uint) public balances; also creates a public state variable. The type maps addresses to unsigned integers. Mappings can be seen as hash tables which are virtually initialized such that every possible key exists from the start and is mapped to a value whose byte-representation is all zeros.
```
function balances(address _account) external view returns (uint) {
    return balances[_account];
}
```
This function can be used to query the balance of a single account.<br>
The line event Sent(address from, address to, uint amount); declares a so-called “event” which is emitted in the last line of the function send. User interfaces can listen for those events being emitted on the blockchain without much cost. As soon as it is emitted, the listener will also receive the arguments from, to and amount, which makes it easy to track transactions.The constructor is a special function which is run during creation of the contract and cannot be called afterwards. It permanently stores the address of the person creating the contract. msg.sender is always the address where the current (external) function call came from.<br><br>
Finally, the functions that will actually end up with the contract and can be called by users and contracts alike are create and send. If cretae is called by anyone except the account that created the contract, nothing will happen. This is ensured by the special function require which causes all changes to be reverted if its argument evaluates to false. The second call to require ensures that there will not be too many coins, which could cause overflow errors later.<br><br>
On the other hand, send can be used by anyone (who already has some of these coins) to send coins to anyone else. If you do not have enough coins to send, the require call will fail and also provide the user with an appropriate error message string.<br>
```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```
