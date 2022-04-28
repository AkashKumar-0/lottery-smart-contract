# lottery-smart-contract
here in this i tried fr the smart contract for lottery events.
1. manager , players and winner initialize karenge
2. ether recive karenge 
3. balance check karnge 
4. winner choose karenge
5. winner choose karne ke liye sha256 or reccak256 ye sab use kr sakte h
```

//SPDX-License-Identifier: MIT

pragma solidity >=0.7.0 <0.9.0;

contract Lottery
{
    address public manager;
    address payable[] private players;
    address payable public winner;
    
    constructor() {
        manager = msg.sender;
    }

    receive() external payable  // buy lootery and save the address
    {
        require(msg.value == 1 ether);            
              players.push(payable(msg.sender));
    }

    function getbalance() public view returns(uint)
    {
        require(msg.sender == manager);
          return address(this).balance ;
    }

    function random() private view returns(uint)
    {
       return uint(keccak256(abi.encodePacked(block.difficulty, block.timestamp , players.length)));
    }

    function chooseWinner() public   // winner keval manager choose kar sakta h
    {
        require(msg.sender == manager, "Only manager can choose the winner");
        require(players.length >= 3 , "participant must be greater than 2");
        uint r = random();
        uint index = r % players.length ;
        winner = players[index];  
        winner.transfer(getbalance()); 
        
        players = new address payable[](0);  // reset all the game 

    }     
}
