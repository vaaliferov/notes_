
```python
import scipy
import joblib
import mlflow
import warnings
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
warnings.filterwarnings('ignore')

from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder
from sklearn.preprocessing import StandardScaler
from sklearn.preprocessing import PolynomialFeatures

from sklearn.model_selection import KFold
from sklearn.model_selection import GridSearchCV
from sklearn.model_selection import StratifiedKFold
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import cross_validate
from sklearn.model_selection import train_test_split

from sklearn.linear_model import LogisticRegression
from sklearn.neighbors import KNeighborsClassifier
from sklearn.ensemble import RandomForestClassifier

from sklearn.metrics import get_scorer
from sklearn.metrics import make_scorer
from sklearn.metrics import accuracy_score
from sklearn.metrics import precision_score
from sklearn.metrics import recall_score
from sklearn.metrics import f1_score

from catboost import CatBoostClassifier
from lightgbm import LGBMClassifier
```

```python
f = lambda x: (x**2, x**3)
df = pd.DataFrame({'x': range(1,10)})
df['x2'], df['x3'] = zip(*df['x'].map(f))
```

```python
def f(r):
    r['x2'] = r['x'] ** 2
    r['x3'] = r['x'] ** 3
    return r

df = pd.DataFrame({'x': range(1,10)})
df = df.apply(f, axis=1)
```

```python
df[:3]
df[1:3]
df[mask]
df['col']
df[['col','col']]

df.iloc[0]
df.iloc[:2]
df.iloc[0:2]
df.iloc[[1,5]]
df.iloc[0:2, 0:4]

df.loc[1, 'col']
df.loc[0:2, 'col']
df.loc[0:2, ['col','col']]
df.loc[[1,5], ['col','col']]
df.loc[mask, 'col']

df['col'].tolist()
df['col'].to_numpy()

df['col'].isna().sum() / len(df)

df = df.drop(columns=['col'])
df.columns = df.columns.drop(['col'])

df['num'].hist()
df['cat'].unique()
df['cat'].value_counts()
df['cat'].value_counts().to_dict()
df['col'].apply(type).value_counts()

df['col'] == 3
df['num'] >= 180
df['cat'].isin([1,2,3])
np.sum((df['num'] >= 180) & (df['num'] <= 190))
np.sum(pd.to_datetime(df['col']).dt.year == 1988)

df.sort_values(by='date')['date'].diff().max().days

df['col'].astype(int)
df['str'].str.split().str[0]
df['str'].str.contains('str')

df.sort_values(by=['num','num'], ascending=[False,True])

df.duplicated(subset=['col','col']).sum()

df.groupby('cat').last()
df.groupby('cat').size() # .reset_index(name='sz')
df.groupby('cat')['num'].mean()
df.groupby('cat')['num'].median()
df.groupby('cat')['num'].apply(list) # .reset_index()
df.groupby('cat')['num'].apply(np.array) # .reset_index()
df.groupby('cat')['num'].apply(np.mean)
df.groupby('cat')['bin'].mean().round(2).to_dict()
df.groupby('cat')['num'].mean().plot.bar(figsize=(4,2))

df['col'] = df.apply(func, axis=1)

df.stack().reset_index()
pd.merge(df1, df2, how='left', on='id')
pd.melt(df, id_vars=['col'], value_vars=cols, value_name='id')

sns.barplot(data=df, x='cat', y='num', ax=ax)
sns.boxplot(data=df, x='num', orient='h', ax=ax)
sns.boxplot(data=df, x='num', y='cat', orient='h', ax=ax)
sns.boxplot(data=df, x='cat', y='num', hue='cat', ax=ax)
sns.countplot(data=df, y='cat', orient='h', ax=ax)
sns.countplot(data=df, x='cat', hue='cat', ax=ax)

sns.histplot(data=df, x='num', bins=n, ax=ax)
sns.histplot(data=df, x='num', hue='cat', bins=n, ax=ax)
sns.displot(data=df, x='num', hue='cat', kde=True, height=3, aspect=4)

sns.heatmap(df[cols].corr().round(1).abs(), cmap='YlGnBu', annot=True)

list(zip(s.index, s))
```

```python
f'{20:02}'
```

```python
import inspect
inspect.signature()
```

```python
import spacy

sp = spacy.load('en_core_web_sm')
doc = sp('I went to a store')

[t.pos_ for t in doc]
[t.lang_ for t in doc]
[t.norm_ for t in doc]
[t.lemma_ for t in doc]
```

```python
import nltk

text = 'I went to a store'
toks = nltk.word_tokenize(text)

def get_tags(toks):
    tags = nltk.pos_tag(toks)
    d = {'J':'a','V':'v','N':'n','R':'r'}
    return [d.get(t[1][0], 'n') for t in tags]

tags = get_tags(toks)
lm = nltk.stem.WordNetLemmatizer().lemmatize
[lm(tok, pos=tag) for tok, tag in zip(toks, tags)]
```

```python
nltk.pos_tag(['went','better']) # [('went', 'VBD'), ('better', 'JJR')]
nltk.word_tokenize('test, 3.88') # ['test', ',', '3.88']
nltk.wordpunct_tokenize('test, 3.88') # ['test', ',', '3', '.', '88']
nltk.stem.PorterStemmer().stem('rocks') # 'rock'
nltk.stem.WordNetLemmatizer().lemmatize('corpora') # 'corpus'
nltk.stem.WordNetLemmatizer().lemmatize('went', pos='v') # 'go'
nltk.stem.WordNetLemmatizer().lemmatize('better', pos='a') # 'good'
nltk.corpus.stopwords.words('english')[:5] # ['i', 'me', 'my', 'myself', 'we']
'test5'.isalnum(), 'test#'.isalnum() # (True, False)
```

```python
toks = ['this','is','a','test']
vocab = {t: i for i, t in enumerate(sorted(toks))}
vocab # {'a': 0, 'is': 1, 'test': 2, 'this': 3}
```

```python
import nltk
import random
# !pip3 install sparsesvd
from sparsesvd import sparsesvd
from sklearn.feature_extraction.text import TfidfVectorizer

alphabet = 'abcdefghijklmnopqrstuvwxyz'
gen_word = lambda: ''.join(random.sample(alphabet, 10))
gen_doc = lambda: ' '.join([gen_word() for i in range(100)])

params = {'tokenizer': nltk.word_tokenize, 
          'ngram_range': (1,1), 'min_df': 1}

docs = [gen_doc() for i in range(1000)]
vectorizer = TfidfVectorizer(**params)
mat = vectorizer.fit_transform(docs)
ut, s, vt = sparsesvd(mat.tocsc(), 100)
k, v = vectorizer.get_feature_names_out(), vt.T
embs = {k[i]: v[i] for i in range(len(v))}
embs[list(embs.keys())[0]].shape # (100,)
```

```python
import nltk
# !pip3 install gensim
from gensim.models import Word2Vec
docs = ['doc 1', 'doc 2', 'doc 3']
toks = [nltk.word_tokenize(d) for d in docs]

wv = Word2Vec(toks, min_count=1, vector_size=100).wv

k, v = wv.index_to_key, wv.vectors
v = (v - v.mean(axis=0)) / v.std(axis=0)
embs = {k[i]: v[i] for i in range(len(v))}
embs['doc'].shape # (100,)
```

```python
import nltk
from gensim.models import Word2Vec
import gensim.downloader as gensim_downloader
# https://github.com/RaRe-Technologies/gensim-data

name = 'glove-wiki-gigaword-100'
wv = gensim_downloader.load(name)
k, v = wv.index_to_key, wv.vectors
v = (v - v.mean(axis=0)) / v.std(axis=0)
embs = {k[i]: v[i] for i in range(len(v))}
embs['cat'].shape # (100,)
```

```python
import numpy as np
toks = nltk.word_tokenize('test doc')
doc_embs = [embs[t] for t in toks if t in embs]
np.vstack(doc_embs).mean(axis=0).shape
```

```python
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import PolynomialFeatures
```

```python
cat_cols = ['c1', 'c2']
num_cols = ['c3', 'c4']

clf = ('clf', LogisticRegression())
ohe = ('ohe', OneHotEncoder(), cat_cols)
sca = ('sca', StandardScaler(), num_cols)
pre = ('pre', ColumnTransformer([ohe, sca])) # remainder='passthrough'

model = Pipeline([pre, clf])
model.fit(X_train, y_train)
model.predict(X_test)
```

```python
from sklearn.model_selection import KFold
from sklearn.model_selection import GridSearchCV
from sklearn.model_selection import cross_validate
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import train_test_split
```

```python
train_df, valid_df = train_test_split(df, test_size=0.1, random_state=42)

kfold = KFold(n_splits=5, shuffle=True, random_state=42)
cv_scores = cross_val_score(model, X_train, y_train, scoring=scorer, cv=kfold, n_jobs=-1)

space = {'clf__C': [0.01, 0.1, 1]}
kfold = KFold(n_splits=5, shuffle=True, random_state=42)
search = GridSearchCV(model, space, scoring=scorer, cv=kfold, n_jobs=-1)
result = search.fit(X_train, y_train) # result.best_score_, best_params_
search.best_estimator_.predict(X_valid)
```

```python
from sklearn.metrics import f1_score
from sklearn.metrics import recall_score
from sklearn.metrics import precision_score
from sklearn.metrics import accuracy_score

accuracy_score(y_valid, model.predict(X_valid))
```

```python
df['target'] = df['target'].map({'M': 1, 'R': 0})
```

```python
scaler = StandardScaler() # MinMaxScaler()
X_train = scaler.fit_transform(X_train)
X_valid = scaler.transform(X_valid)
```

```python
from sklearn.cluster import KMeans
clusters = KMeans(2).fit_predict(X_train)
```

```python
from sklearn.decomposition import PCA
X2_train = PCA(n_components=2).fit_transform(X_train)
```

```python
df.drop(columns=cols_to_drop, inplace=True)
df.dropna(subset=cols, how='all', inplace=True)
df.duplicated(cols)
```

```python
model.intercept_, model.coef_
```

```python
correlation_matrix = DF_TRAIN[cols].corr()
display(correlation_matrix)
sns.heatmap(correlation_matrix, annot=True);
```

```python
plt.figure(figsize=(4, 4))
plt.plot([1,2,3,4,5,6,7,8,9])
plt.scatter(x, y, c=clusters, cmap='bwr')
```

```python
sns.pairplot(DF_TRAIN[cols], plot_kws={'s': 7, 'alpha': 0.5});
```

```python
c = DF_TRAIN.corr(method='pearson')
m = np.triu(np.ones(c.shape), k=1).astype(bool)
c.where(m).stack().sort_values(ascending=False, key=np.abs)[:20]
```

```python
fig, ax = plt.subplots(figsize=(12,10))
sns.heatmap(DF_TRAIN.corr(), annot=False, vmin=-1, vmax=1, ax=ax);
```

```python
fig, ax = plt.subplots(figsize=(13,3))
sns.heatmap(df.isna().transpose(), cmap='YlGnBu', cbar=False, ax=ax);
```

```python
df['Cabin'].apply(type).value_counts()
```

```python
s = df['PassengerId']
s.min(), s.max(), len(s.unique()), len(s)
```

```python
df['Survived'].value_counts(dropna=False).to_dict()
df['Sex'].value_counts(dropna=False).to_dict()
```

```python
Q1, Q3 = c.quantile(0.25), c.quantile(0.75)
IQR = Q3 - Q1; low = Q1 - 1.5 * IQR; high = Q3 + 1.5 * IQR
```
