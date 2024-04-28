 // SPDX-License-Identifier: MIT LICENSE
pragma solidity ^0.8.17;

contract Agriculture {
    struct Product {
        address owner;
        string title;
        string description;
        uint256 target;
        uint256 deadline;
        uint256 amountCollected;
        string image;
        address[] buyers;
        uint256[] amounts;
    }

    mapping(uint256 => Product) public products;

    uint256 public numberOfProducts = 0;

    function list(address _owner, string memory _title, string memory _description, uint256 _target, uint256 _deadline, string memory _image) public returns (uint256) {
        Product storage product = products[numberOfProducts];

        require(product.deadline < block.timestamp, "The deadline should be a date in the future.");

        product.owner = _owner;
        product.title = _title;
        product.description = _description;
        product.target = _target;
        product.deadline = _deadline;
        product.amountCollected = 0;
        product.image = _image;

        numberOfProducts++;

        return numberOfProducts - 1;
    }

    function buyToProduct(uint256 _id) public payable {
        uint256 amount = msg.value;

        Product storage product = products[_id];

        product.buyers.push(msg.sender);
        product.amounts.push(amount);

        (bool sent,) = payable(product.owner).call{value: amount}("");

        if(sent) {
            product.amountCollected = product.amountCollected + amount;
        }
    }

    function getBuyers(uint256 _id) view public returns (address[] memory, uint256[] memory) {
        return (products[_id].buyers, products[_id].amounts);
    }

    function getProducts() public view returns (Product[] memory) {
        Product[] memory allProducts = new Product[](numberOfProducts);

        for(uint i = 0; i < numberOfProducts; i++) {
            Product storage item = products[i];

            allProducts[i] = item;
        }

        return allProducts;
    }
}
