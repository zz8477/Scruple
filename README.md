# Smart contract vulnerable detection

## Task Definition

Detect timestamp vulnerabilities in smart contract.

## Dataset

The dataset we use is [SmartBugs Wild Dataset](https://github.com/smartbugs/smartbugs-wild/tree/master/contracts) and filtered following the paper [Empirical Review of Automated Analysis Tools on 47,587 Ethereum Smart Contracts](https://arxiv.org/abs/1910.10601).

The tools analysis results we use is [Vulnerability Analysis of Smart Contracts using SmartBugs](https://github.com/smartbugs/smartbugs-results) and filtered following the paper [Empirical Review of Automated Analysis Tools on 47,587 Ethereum Smart Contracts](https://arxiv.org/abs/1910.10601).

### Data Format

1. dataset/data.jsonl is stored in jsonlines format. Each line in the uncompressed file represents one contract.  One row is illustrated below.

   - **contract:** the smart contract

   - **idx:** index of the contract
  
   - **address:** the smart contract's address
  
2. dataset/sol_map_contracts.jsonl is stored in jsonlines format. Each line in the uncompressed file represents the vulnerablities information about one or more contreacts in one solidity file found by different tools.  One row is illustrated below.

   - **address:** the solidity file address
  
   - **tools:** the static analysis tools and their detection information 
     - **<tool_name>:** the name of tool
       - **<contract_name>:** the name of contracts in solidity file
       - **<flag\>:** indicates whether this contract is detected as vulnerable by the tool 
  
 

3. train.txt/valid.txt/test.txt provide examples, stored in the following format:    **idx	label**

### Data Statistics

You can get data using the following command.

```
cd dataset
unzip data.zip
```

## Evaluator

We provide a script to evaluate predictions for this task, and report F1 score

### Dependency

- python version: python3.7.4
- pip3 install torch
- pip3 install transformers
- pip3 install tree_sitter
- pip3 sklearn


### vulnerability detection

```shell
cd parser
bash build.sh
cd ..
python detect.py dev
```

### Evaluation

```shell
python evaluator/evaluator.py -a dataset/test.txt -p saved_models/predictions.txt 2>&1| tee saved_models/score.log
```

## Result

The results on the test set are shown as below:

| Method        |  Recall   | Precision |    F1     | 
| ------------- | :-------: | :-------: | :-------: | 
| slither       |   0.78    |   0.82    |   0.84    | 
| manticore     |   0.50    |   0.57    |   0.50    |  
| osiris        |   0.51    |   0.53    |   0.52    |  
| oyente        |   0.52    |   0.55    |   0.52    |  
| smartcheck    |   0.50    |   0.74    |   0.51    |
| GCN           |   0.76    |   0.68    |   0.72    | 
| Vanilla-RNN   |   0.45    |   0.52    |   0.46    |  
| LSTM          |   0.59    |   0.50    |   0.54    |  
| GRU           |   0.59    |   0.49    |   0.54    |  
| DR-GCN        |   0.79    |   0.71    |   0.75    |
| TMP           |   0.84    |   0.75    |   0.79    |  
| CGE           |   0.88    |   0.87    |   0.88    |  
| Scruple       | **0.96**  | **0.90**  | **0.93**  | 
