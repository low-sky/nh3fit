# nh3fit
This IDL library implements ammonia fitting using the method originally developed in [Rosolowsky et al. (2008)](http://adsabs.harvard.edu/abs/2008ApJS..175..509R) and refined in [Dunham et al. (2011)](http://adsabs.harvard.edu/abs/2011ApJ...741..110D).  The work has been reimplemented in [pyspeckit](http://pyspeckit.bitbucket.org/html/sphinx/index.html) and improvements and refinements are being developed through that channel.

This code relies on [mpfit](https://www.physics.wisc.edu/~craigm/idl/fitting.html) and probably a lot of other things you can find in [my core IDL libraries](https://github.com/low-sky/idl-low-sky/).

`nh3fit` is a code that forward models the ammonia spectrum with fine structure components given a uniform slab of gas.  This version assumes that all the lines are characterized by the same excitation temperature and that the different states are populated according to thermodynamic equilibrium within the ortho- and para-ammonia ladders.  

Calling `nh3fit` uses the main routine `nh3fit.pro`, which operates on vectors of frequency and main beam brightness temperature (i.e., corrected for atmospheric opacity and main beam efficiency, as is appropriate for a beam-filling slab).  

	IDL> nh3fit, nu, Tmb, s = output
	
where `nu` and `Tmb` are the frequency in GHz and the main beam temperature in K respectively.  These arrays should be the same length.  `output` is then a structure with fields that contain the optimal physical parameters.

If you want to create a model spectrum for overplotting, you call the `modelspec` routine with the optimized parameters.  

	IDL> Tmb_bestfit = modelspec(nu, output.model)
	
Note that the fit can make use of embedded information about a frequency switch in the data (i.e., accounting for the off components of a simple frequency switch).  This is controlled by setting the `!NH3` system variable with the IDL routine `defsysv`.  The tag `freqswthrow` sets the magnitude of the throw in MHz (because that's how the GBT defines the throw).


