# aumetacrafters-avax

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MyToken {

Public variables

    string public tokenName = "Tokneneng";
    string public tokenSymbol = "Token";
    uint256 public totalSupply = 100000;
    uint public _initialSupply;
    
Mapping

    mapping(address => uint256) public balances;
    
Event

    event Mint(address indexed to, uint256 value);
    event Burn(address indexed from, uint256 value);
    event TokensTransferred(address indexed from, address indexed to, uint256 value);

Constructor

    constructor() {
        _initialSupply = totalSupply;
        balances[msg.sender] = _initialSupply;
    }
    
Mint Functions to mint Tokens

    function mint(address _to) public {
        totalSupply += 100;
        balances[_to] += 100;
        emit Mint(_to, 100);
    }

Burn Function to Burn tokens

    function burn() public {
        require(balances[msg.sender] >= 100, "Insufficient balance to burn");
        totalSupply -= 100;
        balances[msg.sender] -= 100;
        emit Burn(msg.sender, 100);
    }

Transfer function with require, assert and revert features

    function transfer(address _from, address _to, uint256 _value) public {
        require(_from != address(0), "Invalid sender address");
        require(_to != address(0), "Invalid recipient address");
        require(_value > 0, "Invalid transfer amount");

        if (balances[_from] < _value) {
            revert("Insufficient balance to transfer");
        }

        uint256 senderBalanceBefore = balances[_from];
        uint256 receiverBalanceBefore = balances[_to];

        balances[_from] -= _value;
        balances[_to] += _value;

        assert(balances[_from] + _value == senderBalanceBefore);
        assert(balances[_to] - _value == receiverBalanceBefore);

        emit TokensTransferred(_from, _to, _value);
    }
}
