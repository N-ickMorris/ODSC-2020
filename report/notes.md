### The Future of MLOps and How Did We Get Here?
### Chris Sterry | Vice President of Operations | Dotscience
- They started this coming from the DevOps mindset (DevOps for ML)
- They started an online community for [MLOps](https://mlops-community-slack.com/) weekly talks with guests

*MLOps requirements*
1. Reproducible: You can re-train a model built 9 months ago and reproduce it within tolerance
2. Accountable: You can recover the raw data, training data, and build procedure of every model in production
3. Collaborative: You can prove someone else's model works correctly without talking to them
4. Continuous: You can statistically monitor your models to correct abnormal behavior in production

"In the deployment what I’ve typically found in talking to ML teams is right now it seems like a data scientist is expected to be a software engineer and, in some organizations, to be a master of DevOps as well. And often the gap between data scientist and DevOps and software engineering is pretty broad; And when you have to wear all those hats it’s: how do you simplify that? Then from a monitoring standpoint, I always say that if you knew the right answer then you wouldn't need machine learning. Models can go wonky quickly in production without normal monitoring techniques, and so you need to do statistical monitoring."

*Operations*
```txt
DevOps
------
* Focused on code commits
Code──>──Test──>──Deploy
  |                 |
  └──<──Monitor──<──├

- Code: source code and unit test
- Test: behavior test
- Deploy: build and re-versioning
- Monitor: track model quality over time

MLOps
-----
* Focused on all the moving pieces that create data sets and models
Data runs   Code   Parameters
 |  |        |      |
 |  └────────└──────└──>──Model runs──>──Models/metrics──>──Deploy
 |                                                            |
 └─────────────<────────────Monitor──────────<────────────────├

- Data runs: raw/engineered data
- Code: source code and unit test
- Parameters: meta data and hyperparameter values
- Model runs: training models
- Models/metrics: final models and goodness of fit
- Deploy: behavior test, build, and re-versioning
- Monitor: track model quality over time

MLOps Life Cycle
----------------
1. Raw Data, Engineered Data: Building engineering pipelines for preparing data
2. Training, Development: Fine tuning models for official release
3. Deploy Model, Production, Statistical Monitoring: Pushing new model versions through CI/CD and being alerted on issues as models depreciate
               Raw Data
                  |
 Training──<──Engineered Data──<──Real-time decisions
     |                                           |
Development──>──Deploy Models──>──Production──>──├
     |                                |  |
     └──<──Statistical Monitoring──<──├  └──<──Real-time users
```

*Provenance*
A provenance (precedence) diagram is a way to track the order of all operations for constructing a model. Storing the logic of this diagram in a program would allow us to query for any version of the data and model during construction.
