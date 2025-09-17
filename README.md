# contract-MockBtcOracle

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

interface AggregatorV3Interface {
    function decimals() external view returns (uint8);
    function latestRoundData() external view returns (
        uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound
    );
}

contract BitBits is ERC20, Ownable {
    // --- ALL DECLARATIONS HERE ---
    event TokensMinted(address indexed to, uint256 ethDeposited, uint256 btcPrice, uint256 amount);
    address public constant ETH_USD_ORACLE = 0x694AA1769357215DE4FAC081bf1f309aDC325306;
    address public constant BTC_USD_ORACLE = 0x1b44F3514812d835EB1BDB0acB33d3fA3351Ee43;
    uint8 public constant BTC_DECIMALS = 8;
    uint8 public constant TOKEN_DECIMALS = 18;
    uint256 public constant INITIAL_SUPPLY = 70_000_000 * 10**TOKEN_DECIMALS;
    uint256 private constant DECIMALS_FACTOR = 10**TOKEN_DECIMALS;
    uint256 private constant MAX_SLIPPAGE_BPS = 100;
    error InvalidAmount(uint256 amount);
    error OracleError(string message);

    constructor() ERC20("BitBits", "BitBtc") Ownable(msg.sender) {
        _mint(msg.sender, INITIAL_SUPPLY);
    }

    // ... rest of your functions (mintBacked, getBtcPriceAge, etc.) ...
}

// Separate mock contract (correctly outside BitBits)
contract MockBtcOracle {
    function latestRoundData() external view returns (uint80, int256, uint256, uint256, uint80) {
        return (0, 130000 * 1e8, 0, block.timestamp, 0);
    }
    function decimals() external pure returns (uint8) { return 8; }
}
