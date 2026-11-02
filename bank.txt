// SPDX-License-Identifier: MIT
pragma solidity ^0.8.38;

// Smart contract for a simple bank account
contract BankAccount {

    // Store the balance of each wallet address
    mapping(address => uint) private balances;

    // Function to deposit Ether into the account
    function deposit() public payable {

        // Check that the deposited amount is greater than zero
        require(msg.value > 0, "Deposit amount must be greater than zero");

        // Add deposited Ether to the sender's balance
        balances[msg.sender] += msg.value;
    }

    // Function to withdraw Ether from the account
    function withdraw(uint amount) public {

        // Check that withdrawal amount is greater than zero
        require(amount > 0, "Withdrawal amount must be greater than zero");

        // Check whether the user has sufficient balance
        require(balances[msg.sender] >= amount, "Insufficient balance");

        // Deduct amount before transferring Ether
        balances[msg.sender] -= amount;

        // Transfer Ether to the sender's wallet
        (bool success, ) = payable(msg.sender).call{value: amount}("");

        // Check whether the transfer was successful
        require(success, "Transfer failed");
    }

    // Function to display the user's account balance
    function getBalance() public view returns (uint) {

        // Return balance of the calling wallet address
        return balances[msg.sender];
    }
}