# Numpy
## Create array and properties
```python
np.eye(c)[arr] # 2-D array with ones on the diagonal and zeros elsewhere, 1's pos can be determined by indexing with array
np.genfromtxt(lul,delimiter=',') # read csv with numpy 
pd.read_csv(path)
np.random.randint(2, high=3 size=9) # 2 is the low value
np.zeros(size, dtype=int) # fill with zeros
arr.shape
arr.ndim
np.shape(X)
```
## Random
```python
np.random.seed(42) # random number with seed
np.random.normal(0.0, 1, (self.n_input, self.n_output))
np.random.shuffle(dataset) # 
```
## Filter array or transform/modify
```python
np.nonzero(arr) # returns the non zero elements
np.where(arr==1)[1] # filer array results are found in first index
arr=arr !=0 #array can be filetr this way to
np.unique(y)
np.delete(y,np.where(x < 0.0)[0],axis=0)
arr[arr<num]=-1 # replace with condition
np.ndarray.flatten(arr) # to 1D array
arr.reshape((2,3)) # change shape of array
arr.astype("float32")
np.round(arr,to)
np.array(arr[:]==arr2[:]) # compare arrays outputs mask
np.fill_diagonal(arr, 4, wrap=True) # középső értéeke 4
np.transpose(arr) # [1,2],[3,4] -> [1,3],[2,4]
np.fliplr(arg) # swap columns 
addTen = np.vectorize(add)
arr = addTen(arr)

```
## NAN handling
```python
np.nanmean(x,axis=0),np.nanvar(x,axis=0) # nan helyére átlag rakás
x[np.isnan(x)] = 3.5
df.dropna() # droprows with missing values
```
## Multi array operations
```python
np.column_stack(arr1,arr2)  # Stack 1-D arrays as columns into a 2-D array.
np.vstack() # Stack arrays in sequence vertically (row wise).
np.matmul(X,self.W) # Matrix product of two arrays
x,y = dataset[:,:-1],dataset[:,-1] # x: every column except last y: last
np.concatenate((X, Y), axis=1)
```
## Masking
```python
mask = (clusters == cluster)
target = digits.target[mask]
np.ma.masked_values(arr,1).mask # make mask 1 for true others false
```
## One value output operations and others
```python
np.sum(residuals ** 2)
np.mean(x,axis=0) # axis determines if columns or rows
np.mean(y_train-y_pred) # mean of the y_train-y_pred values
np.maximum(0, x)
np.amax(arr) # max in array or along axis
np.arange(start_date, end_date, dtype='datetime64[D]') # arrange dates
np.datetime64('today') 
np.argmin(distances)
```

# Pandas
```python
pd.DataFrame(inputDict) # create datagrame from dict
pd.DataFrame(iris.data, columns=iris.feature_names) # data and colum names can be defined seperatly
df.mean() # 
new_df=df.copy() # copy df
df.isnull()
df.mean() # mean all columns
df[name] # get column by name
df.tail(2) # get bottom records
df.head(2) # get top 2 records
np.var(x,axis=0) # variance
data = pd.read_csv("data/Iris.csv",skiprows=1, header=None, names=col_name)
df.sort_values("area") # sort values by columns
```
## Modify collums
```python
df.groupby(["parental level of education"]) # the paramter column will be the "key" like sql
new_df["density"]=density # create new column, like in a dict
density= new_df["population"] / new_df["area"] # calc new colums by other columns
new_df["age"]=np.random.randint(18,67,size=len(new_df)) # columns values by random numbers
df["variety"][df["variety"]==0]="Setosa" # change column value by condition
data.drop(labels=['sepal length (cm)'],axis=1)
df.iloc[::-1] # idexing rows or slicing
```
## Filter records
```python
df.columns.to_series().apply(UpperNotE) # apply to fucntion to series item by item
np.array([row for row in dataset if row[feature_index]<=threshold])
```
# Sklearn
## Clustering
```python
from sklearn.cluster import KMeans
from sklearn.datasets import make_blobs
# use the make_blobs function to create the X and y_ture variables
# let n_samples = 500, centers = 4, cluster_std = 0.40, random_state = 0
X, y_true = make_blobs(n_samples = 500, centers = 4,cluster_std = 0.99, random_state = 1)
kmeans = KMeans(n_clusters = 4)
kmeans.fit(X)
y_kmeans = kmeans.predict(X)
centers = kmeans.cluster_centers_
plt.scatter(X[:, 0], X[:, 1], c = y_kmeans, s = 50, cmap = 'viridis')
# color it to red, let s=200 and set alpha to 0.
plt.scatter(centers[:, 0], centers[:, 1], c = 'red', s = 200, alpha = 0.5)
plt.show()
```
### Gaussian Mixture Model
```python
data = pd.read_csv('Clustering_gmm.csv')
# training gaussian mixture model 
from sklearn.mixture import GaussianMixture
gmm = GaussianMixture(n_components=4)
gmm.fit(data)

#predictions from gmm
labels = gmm.predict(data)
frame = pd.DataFrame(data)
frame['cluster'] = labels
frame.columns = ['Weight', 'Height', 'cluster']

color=['blue','green','cyan', 'black']
for k in range(0,4):
    data = frame[frame["cluster"]==k]
    plt.scatter(data["Weight"],data["Height"],c=color[k], s=30)
plt.show()
```
## SVM
```python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.metrics import precision_score
from sklearn.metrics import recall_score
from sklearn.model_selection import GridSearchCV
from sklearn.svm import SVC
cancer=load_breast_cancer()
print(cancer["feature_names"])
df=pd.DataFrame(columns=cancer.feature_names, data=cancer.data)
df["target"]=cancer.target
X_train, X_test, y_train, y_test = train_test_split(cancer.data, cancer.target, test_size=0.20, random_state=42)
d=SVC(kernel='linear')
d.fit(X_train,y_train)
pred=d.predict(X_test)

truePos=(pred == y_test).sum()
acc=truePos/len(y_test)

```
## Gridsearch
```python
param_grid = {'C': [0.1, 1, 10, 100, 1000], # try out parameters which yields the best result
	'gamma': [1, 0.1, 0.01, 0.001, 0.0001],
	'kernel': ['rbf']}
g=GridSearchCV(estimator=SVC(), param_grid=param_grid,verbose=1)
```
## LinearRegression
```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression, LogisticRegression
from sklearn.metrics import mean_squared_error
iris=sklearn.datasets.load_iris()
data=pd.DataFrame(iris.data, columns=iris.feature_names)
y=data['sepal length (cm)']
X=data.drop(labels=['sepal length (cm)'],axis=1)
train_test_split(X, y, test_size=0.2, random_state=42)
X_train, X_test, y_train, y_test=LinearRegression().fit(X_train, y_train)

LogisticRegression(solver='liblinear', random_state=42).fit(X_train, y_train)
# or
LinearRegression().fit(X_train, y_train)
model.predict(X_test)
reg.evaluate(reg.X_train,reg.y_train)
```
## Metrics
```python
from sklearn.metrics import accuracy_score
accuracy_score(Y_test, Y_pred) # Accuracy classification score

from sklearn.metrics import confusion_matrix
conf_matrix = confusion_matrix(digits.target,labels) # evaluate the accuracy of a classification

from sklearn.metrics import mean_squared_error
mean_squared_error(y_test, y_pred)
```
# Mathplyplot
```python
import matplotlib.pyplot as plt

# plot linear regression with line
plt.scatter(X_test, y_test)
plt.plot([min(X_test), max(X_test)], [min(y_pred), max(y_pred)], color='red')
plt.show()

# Plot the data points and cluster centers
plt.scatter(centers[:, 0], centers[:, 1],c=digits.target_names)
plt.show()


fig, ax = plt.subplots()
ax.set_title('Actual vs Predicted Target Values')
ax.set_xlabel('Actual')
ax.set_ylabel('Predicted')
ax.scatter(y_test, y_pred)
plt.show()

ax.hist(g['gender'].count().values,g.groups.keys()) # histogram
ax.set_xlabel="Writing Score"
ax.set_ylabel="Number of Students"
ax.set_title("Distribution of Writing Scores")

fig, ax = plt.subplots() # pie charts
ax.set_title("Proportion of Students by Race/Ethnicity")
ax.pie(g.count()["lunch"].values, labels=g.groups.keys(),autopct='%1.1f%%')
```
