# lottery-smart-contract
here in this i tried fr the smart contract for lottery events.
//SPDX-License-Identifier: MIT

pragma solidity >=0.7.0 <0.9.0;

contract Lottery
{
    address public manager;
    address payable[] public players;
    
    constructor() {
        manager = msg.sender;
    } 
    
    function alreadyEntered() view private returns(bool)
    {
        for(uint i=0; i<players.length; i++)
        {
             if(msg.sender == players[i])
               return true;
        }
        return false;
    }

    function enter() payable public

    {
         require(msg.sender != manager,"Manager can't Enter ");
         require(alreadyEntered() == false,"Player already Entered "); 
         require(msg.value >= 1 ether , "Minimun amount must be paid");      
         players.push(payable(msg.sender));   // jisne bhi lottery kharida h uska naam store karlo                                                                                         
    }
    
    function random() view private returns(uint)
    {
       return  uint(sha256(abi.encodePacked(block.difficulty,block.number,players)));  / we use sha256 function to choose winner
    }

    function pickwiiner() public
    {
       require(msg.sender == manager , "Only manager can pick winner ");
       uint index =random()%players.length; 
       address contractAddress = address(this);
       players[index].transfer(contractAddress.balance);
       players = new address payable[](0);
    }

    function getplayers() view public returns(address payable[] memory)
    {
        return players;
    }
}
