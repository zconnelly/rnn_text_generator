RNN Text Generator
==================

### Prerequisites

- pip
- virtualenv
- python

### Install requirements

```
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
```

### Where can I get some training data?

Structured text works best. Text that follows patterns. We can start with bash history

```bash
cp ~/.bash_history training_data.txt
```

Now we also need start and end tokens -- which we can do with `sed`

```bash
sed -ie 's/^/START_MESSAGE /g' training_data.txt
sed -ie 's/$/ END_MESSAGE/g' training_data.txt
```

### Sanity Check

Run `cat training_data.txt | head -n 5` and you should see something like this

```
START_MESSAGE cat ~/.bash_history END_MESSAGE
START_MESSAGE vim README.md  END_MESSAGE
START_MESSAGE vim ~/.tmux.conf  END_MESSAGE
START_MESSAGE cat ~/.bash_history training_data.txt END_MESSAGE
START_MESSAGE ls END_MESSAGE
```

### Train it

Uncomment the following section of `rnn.py`
```python
# train
"""
np.random.seed(10)
model = RNN(vocabulary_size)
train_with_sgd(model, x_train, y_train, nepoch=100, evaluate_loss_after=1)
"""
```

Then run `python rnn.py`

You should see something like this...

```bash
zac@macbook ~/personal_projects/rnn_text_generator $ vim rnn.py
Expected Loss for random predictions: 8.142936
Actual loss: 8.142497
2017-11-20 14:23:30: Loss after num_examples_seen=0 epoch=0: 8.142977
2017-11-20 14:26:46: Loss after num_examples_seen=20028 epoch=1: 3.109136
```

Once it's trained for a while... exit with control C

You should now see a `model.p` file generated

```bash
# ls -la | grep "model.p"
-rw-r--r--   1 zacmimi  staff  12797581 Nov 20 14:26 model.p
```

### Generate some Text!

back in `rnn.py` comment out the `train` section that you uncommented earlier

Then uncomment this part

```python
# load
model = pickle.load(open("model.p", "rb"))
```

You can now generate likely bash commands by running `rnn.py`

```
zac@macbook ~/personal_projects/rnn_text_generator $ python rnn.py
Expected Loss for random predictions: 8.142936
Actual loss: 8.142497
vim src / modules / test

vim . . . /

vim src / grep

vim src / . /

vim projects / . .

vim tests / .

vim - / . . py

vim ~ / .

vim . / .
```

it turns out that it has learned which text editor I prefer
