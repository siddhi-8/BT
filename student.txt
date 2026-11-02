// SPDX-License-Identifier: MIT
pragma solidity ^0.8.38;

// Smart contract to store student information
contract StudentData {

    // Structure to store student details
    struct Student {
        uint rollNo;
        string name;
        uint age;
        string department;
    }

    // Dynamic array to store multiple students
    Student[] public students;

    // Event generated when a student is added
    event StudentAdded(uint rollNo, string name);

    // Event generated when fallback function is called
    event FallbackCalled(address sender, uint amount);

    // Function to add a new student
    function addStudent(
        uint _rollNo,
        string memory _name,
        uint _age,
        string memory _department
    ) public {

        require(_rollNo > 0, "Invalid roll number");
        require(_age > 0, "Invalid age");

        students.push(
            Student(_rollNo, _name, _age, _department)
        );

        emit StudentAdded(_rollNo, _name);
    }

    // Function to retrieve student details
    function getStudent(uint index)
        public
        view
        returns (uint, string memory, uint, string memory)
    {
        require(index < students.length, "Student not found");

        Student memory s = students[index];

        return (s.rollNo, s.name, s.age, s.department);
    }

    // Function to get total number of students
    function getStudentCount() public view returns (uint) {
        return students.length;
    }

    // Receive function to accept plain Ether
    receive() external payable {
        emit FallbackCalled(msg.sender, msg.value);
    }

    // Fallback function for unmatched function calls
    fallback() external payable {
        emit FallbackCalled(msg.sender, msg.value);
    }
}