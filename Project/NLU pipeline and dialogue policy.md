![[Pasted image 20241221164731.png]]
![[Pasted image 20241221164831.png]]
![[Pasted image 20241221165026.png]]
EXample:
![[Pasted image 20241221165044.png]]

### Tokenizers: 
![[Pasted image 20241221165230.png]]
Ex: wide space tokenizer.
### Featurizers
![[Pasted image 20241221165346.png]]

### Classifiers
![[Pasted image 20241221165447.png]]

### Entity Extractors
![[Pasted image 20241221165534.png]]


----

# Training Policies
![[Pasted image 20241221170106.png]]

![[Pasted image 20241221170515.png]]

## Types of policies
![[Pasted image 20241221170634.png]]

#### Rule Based
![[Pasted image 20241221170832.png]]

#### Memoization Policy
![[Pasted image 20241221170939.png]]

#### TED Policy 
![[Pasted image 20241221170957.png]]

# Custom Actions
![[Pasted image 20241221172004.png]]
### SET SLOT
![[Pasted image 20241221180610.png]]
get slot:
```python
tracker.get_slot("location");
```


# FORMS
nlu.yaml
![[Pasted image 20241221183259.png]]
rules.yml -> activate and deactivate form
![[Pasted image 20241221183335.png]]
Domain.yml
1. add intent request_pizza_form
2. add entities pizza_size, pizza_type
3. ![[Pasted image 20241221183637.png]]
4. ![[Pasted image 20241221183703.png]]
5. ![[Pasted image 20241221183804.png]]
6. ![[Pasted image 20241221183947.png]] The naming convenction for the ValidateFn should be validate_<slot_name>
![[Pasted image 20241221184244.png]]


