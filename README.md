# Example FeatureForm setup - illustrating unexpected output

## Prerequisites

- [poetry](https://python-poetry.org/)

## How to

### Set up feature store

```bash
git clone https://github.com/samuell/ff-unexpected-output.git
cd ff-unexpected-output
poetry shell
poetry init
featureform apply --local
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
