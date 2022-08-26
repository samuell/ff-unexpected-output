# Example FeatureForm setup - illustrating unexpected output

**UPDATE: Fixed now! See the end of the README!**

## Prerequisites

- [poetry](https://python-poetry.org/)
- OR, some other way of installing the dependencies specified in
  `pyproject.toml`, under `[tool.poetry.dependencies]`.

## How to

### Set up feature store

```bash
git clone https://github.com/samuell/ff-unexpected-output.git
cd ff-unexpected-output
poetry shell
poetry install
featureform apply --local defs.py
```

### Run client

```bash
python client.py
```

### Expected output

I would expect to see a single value per time series:

```
-------Foo-------
person    foo
2  samuel  0.002
-------Bar-------
person    bar
2  samuel  0.004
```

Why? Because [the docs](https://docs.featureform.com/getting-started/serving-for-inference-and-training#serving-for-inference) say:

> If a feature has a timestamp, only its most recent value is stored in the
> inference store. It can be thought of as a feature cache for inference time.

### Actual output

Instead what we get is the full time series:

```
-------Foo-------
person    foo
2  samuel  0.002
1  samuel  0.001
0  samuel  0.000
-------Bar-------
person    bar
2  samuel  0.004
1  samuel  0.002
0  samuel  0.001
```

### Update: Fixed now, by upgrading FeatureForm

From the [slack chat](https://featureform-community.slack.com/archives/C022WLBCW91/p1661464903066739?thread_ts=1661289725.628769&cid=C022WLBCW91):

Sterling Dreyer
> Hey Samuel, thanks for the in-depth walkthrough. I believe you mentioned
> something about downgrading packages previously. It seems that poetry is
> pulling version 1.1.1, which is an older version of localmode that has a bug.
> The upgrading to 1.1.10 seems to work
  
Samuel Lampa
> @Sterling Dreyer Ah!! Indeed, you are right. Doh, I didn't realize it did
> downgrade that far. I must have misread the version numbers or something,
> thought it went down just a digit or so. Anyhow, yes, managed to upgrade to
> featureform 1.1.10 after a small modification to the pandas version spect
> (Setting it to ^1.3.5 instead of ^1.4.3), and it works. Again, many thanks!
> :100: (edited) 0
