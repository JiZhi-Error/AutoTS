# Basic Tenants
* Ease of Use > Accuracy > Speed (with speed more important with 'fast' selections)
* Availability of models which share information among series
* All models should be probabilistic (upper/lower forecasts)
* New transformations should be applicable to many datasets and models
* New models need only be sometimes applicable
* Fault tolerance: it is perfectly acceptable for model parameters to fail on some datasets, the higher level API will pass over and use others.
* Missing data tolerance: large chunks of data can be missing and model will still produce reasonable results (although lower quality than if data is available)

## Assumptions on Data
* Series will largely be consistent in period, or at least up-sampled to regular intervals
* The most recent data will generally be the most important
* Forecasts are desired for the future immediately following the most recent data.
* trimmed_mean to AverageValueNaive

# 0.6.16 🇺🇦 🇺🇦 🇺🇦
* export_template added focus_models option
* added OneClassSVM and GaussianMixture anomaly model options
* added plot_unpredictability_score
* added a few more NeuralForecast search options
* bounds_only to Constraint transformer
* updates for deprecated upstream args
* FIRFilter transformer added
* mle and imle downscaled to reduce score imbalance issues with these two in generate score
* SectionalMotif now more robust to forecast lengths longer than history
* new transformer and metric options for SectionalMotif
* NaN robustness to matse
* 'round' option to Constraint
* minor change to mosaic min style ensembles to remove edge case errors
* 'mosaic-profile', 'filtered', 'unpredictability_adjusted' and 'median' style mosaics added
* updated profiler, and improved feature generation for horizontal generalization
* changepoint style trend as an option to GLM and GLS
* added ShiftFirstValue which is only a minor nuance on PositiveShift transformer
* added BasicLinearModel model
* datepart_method, scale, and fourier encodig to WindowRegression
* trimmed_mean and more date part options to SeasonalityMotif
* some additional options to MultivariateRegression
* added ThetaTransformer
* added TVVAR model (time varying VAR)
* added ChangepointDetrend transformer
* added MeanPercentSplitter transformer
* updated load_daily with more recent history
* added support for passing a custom metric

### Unstable Upstream Pacakges (those that are frequently broken by maintainers)
* Pytorch-Forecasting
* Neural Prophet
* GluonTS

### New Model Checklist:
	* Add to ModelMonster in auto_model.py
	* add to appropriate model_lists: all, recombination_approved if so, no_shared if so
	* add to model table in extended_tutorial.md (most columns here have an equivalent model_list)
	* if model has regressors, make sure it meets Simulation Forecasting needs (method=="regressor", fails on no regressor if "User")

## New Transformer Checklist:
	* Make sure that if it modifies the size (more/fewer columns or rows) it returns pd.DataFrame with proper index/columns
	* add to transformer_dict
	* add to trans_dict or have_params or external
	* add to shared_trans if so
	* oddities_list for those with forecast/original transform difference
	* add to docstring of GeneralTransformer
	* add to dictionary by type: filter, scaler, transformer
	* add to test_transform call

## New Metric Checklist:
	* Create function in metrics.py
	* Add to mode base full_metric_evaluation  (benchmark to make sure it is still fast)
	* Add to concat in TemplateWizard (if per_series metrics will be used)
	* Add to concat in TemplateEvalObject (if per_series metrics will be used)
	* Add to generate_score
	* Add to generate_score_per_series (if per_series metrics will be used)
	* Add to validation_aggregation
	* Update test_metrics results
	* metric_weighting in AutoTS, get_new_params, prod example, test
