# MongoDB-Python Driver

## Sample code:

+ Notice: The interaction between the MongoDB database and Tensorflow is done through the `numpy.array()`. Since the interaction is specific to each situation, the following is just one sample code.

```python
import tensorflow as tf
import numpy as np
from pymongo import MongoClient

def initialize_db_col(database, col):
    client      = MongoClient()

    # Access the target database and corresponding collection
    db          = client[database]
    collection  = db[col]

    return collection

def mongodb_to_tensorflow(collection, field):
    # REQUIRE: The target datasets must be imported into the database

    # Fetch the key list in the database
    attr        = ["sepal_length", "sepal_width", "petal_length", "petal_width", "label"]

    # sample      = collection.find_one()
    # attr_cnt    = len(sample) - 1           # number of attributes
    # entry_cnt   = collection.count()        # number of entries

    num_arr     = get_numpy_array(collection, attr, field)
    return num_arr

def tensorflow_to_mongodb(collection, entry):
    # Insert one entry into the collection
    collection.insert_one(entry)

def get_numpy_array(collection, attr, field):
    data = []
    for entry in collection.find():
        l = []
        for i in field:
            l.append(entry[attr[i]])
        data.append(l)
    return np.array(data)

def add_layer(inputs, in_size, out_size, activation_function=None):
    # add one more layer and return the output of this layer
    Weights = tf.Variable(tf.random_normal([in_size, out_size]))
    biases = tf.Variable(tf.zeros([1, out_size]) + 0.1)
    Wx_plus_b = tf.matmul(inputs, Weights) + biases
    if activation_function is None:
        outputs = Wx_plus_b
    else:
        outputs = activation_function(Wx_plus_b)
    return outputs


# train data

field       = (0,1,2,3)
label       = (4,)
database    = "train"
col         = "col"
# Initialize the database and the collection
collection  = initialize_db_col(database, col)
# read the first 4 columns
x_data      = mongodb_to_tensorflow(collection, field)
# read the fifth column
target      = mongodb_to_tensorflow(collection, label)

y_data = np.zeros((len(target),3))

for i in list(range(0, len(y_data), 1)):
    map = {
        0: [1, 0, 0],
        1: [0, 1, 0],
        2: [0, 0, 1],
    }
    y_data[i] = map[target[i][0]]


# print(y_data.shape)
# set placeholder to receive data
# define placeholder for inputs to network  
xs = tf.placeholder(tf.float32, [None, 4])
ys = tf.placeholder(tf.float32, [None, 3])

# define the layers
# add hidden layer inpuy : xs 12unit   
l1          = add_layer(xs, 4, 12, activation_function=tf.nn.relu)
# add output layer input: l1  output  3 classes
prediction1 = add_layer(l1, 12, 3, activation_function=None)
# add softmax layer
prediction  = tf.nn.softmax(prediction1)


# define loss
# the error between prediciton and real data    
loss = tf.reduce_mean(tf.reduce_sum(tf.square(ys - prediction),
                                              reduction_indices=[1]))

cross_entropy = tf.reduce_mean(-tf.reduce_sum(ys * tf.log(prediction),
                                              reduction_indices=[1]))


# choose optimizer     
train_step = tf.train.GradientDescentOptimizer(0.1).minimize(cross_entropy)


# important step, initial
init = tf.global_variables_initializer()  # new feature
sess = tf.Session()
# run
sess.run(init)

# sess.run optimizer
# Initialize the output database and the collection
database    = "train_result"
col         = "bp_iris"
collection  = initialize_db_col(database, col)
for i in range(5000):
    # training train_step & loss , placeholder ,feed give data
    sess.run(train_step, feed_dict={xs: x_data, ys: y_data})
    if i % 20 == 0:
        # to see the step improvement
        # print(sess.run(cross_entropy, feed_dict={xs: x_data, ys: y_data}))
        entry       = {}
        entry[str(i)]    = str(sess.run(cross_entropy, feed_dict={xs: x_data, ys: y_data}))
        tensorflow_to_mongodb(collection, entry)

# Finish Indicator
print("Training Results of BP Algorithm with Iris Datasets have been inserted into the database!")
```



## Reference:

* Basic tutorial: http://www.runoob.com/mongodb/mongodb-tutorial.html
* Official documentation: https://docs.mongodb.com/manual/
* MongoDB-Python Driver: http://api.mongodb.com/python/current/tutorial.html?_ga=1.126571278.205344545.1483513991#tutorial

