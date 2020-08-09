XENON1T S2-only data release
============================

XENON Collaboration, 2020

This release contains data from the analysis described in the paper: 
  * Aprile, E. _et al._ (XENON collaboration), [Phys. Rev. Lett. 123, 251801](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.123.251801) (2019). 

This data release allows researchers to compute limits on their own dark matter models using the XENON1T S2-only data. Please cite our paper if you do so. For questions or comments, contact xenon@lngs.infn.it.


Contents
---------
This data release contains the following files, described below in detail:

  * `README.md`: The file you are reading now
  * `example_analysis.ipynb`: An example jupyter notebook, showing how to derive limits on a dark matter model.
  * `s2_response_nr.csv`: The response matrix of our experiment and analysis to nuclear recoils. Use this to compute the expected number of events for an arbitrary theoretical energy spectrum, e.g. in events / (kg day keV).
  * `s2_response_er.csv`: A similar matrix, for electronic recoil models.
  * `s2_response_n_produced_electrons.csv`: A similar matrix, for spectra expressed as events / (number of produced electrons at the interaction site). Use this if you have your own custom liquid xenon response.
  * `s2_binning_info.csv`: The S2 binning specification. This is needed to interpret the response matrices.
  * `events_after_cuts.csv`: The S2 area and other quantities of the observed events in the search data. You only need this if you want to customize the S2 region of interest (or change the analysis method even more drastically).
  * `events_after_cuts_training.csv`: Similar, for the disjoint training data set. See our paper for more information on this.
  * `er_and_cevns_backgrounds.csv`: The nominal ER and CEvNS backgrounds subtracted in our analysis, expressed as events / S2 bin. Like the previous two files, you usually will not need this.

We also provide two folders:
  * `limits`: This contains the dark matter limit datapoints files we already distributed with the paper. In addition, we provide the datapoints for the dotted lines in figure 5A-C, i.e. the limits assuming the NEST v2.0.1 Qy, cut off at 0.3 keV.
  * `plots`: contains png plots showing the response matrices.

Finally, our raw exposure in this analysis is **0.97678 tonne year**, or 356770 kg days. You will need this as a scale factor when computing the final number of observed events. Note that:
  * This is the exposure for the search data (i.e. 70% of XENON1T's SR1). The training data has exactly 3/7th of this exposure, or 0.41862 tonne year.
  * The _effective_ exposure in this analysis is far lower than the above number, as discussed in the paper, due to strong event selections. All of these effects are included in the response matrices.



s2_response_nr.csv
--------------------

This is the response matrix of our experiment and analysis to nuclear recoil events. Note:
  * We provide the response for nuclear recoils between 0.4 keV and 50 keV. Thus, the conventional 0.7 keV nuclear recoil energy cutoff is NOT applied if you use the response matrix naively.
  * This assumes the default nuclear recoil charge yield Qy, i.e. from https://arxiv.org/abs/1902.11297, not NEST v.2.0.1. If you want to change the Qy or other aspects of the liquid xenon response model, use the s2_response_n_produced_electrons.csv response matrix.
  * The matrix does not sum to one across the S2 bins for any energy, since most events do not pass our event selections. 

Columns:
  * `energy_kev`: Nuclear recoil energy in keV. This is the energy at which we calculated the mono-energetic signal response. Note this is the full energy deposisted by the recoiling particle, including heat (which is undetectable for XENON1T). 
  * `energy_bin_start_kev`: We calculated the response due to a series of single energies. Nonetheless, if you need to consider the energies as a grid of bins, use this as the start point of the bin corresponding to this energy. It is simply the point midway between the current and previous energy in log energy (for the lowest energy, we extrapolated). 
  * `energy_bin_end_kev`: Similar, end point of the energy bin.
  * `s2_bin_000`: fraction of recoils of the energy listed in the first column that cause a signal in the first S2 bin, i.e. [90 - 91.6] PE. You can find the S2 binning in s2_binning_info.csv. For this fraction, we considered all events in our full target mass; that is, the response matrix includes the effect of all event selections.
   * `s2_bin_001`: similar, for the second S2 bin (bin number 1). 
   * ... and so forth.
  
  
s2_response_er.csv
-------------------- 

This file is similar to `s2_response_nr.csv`, but describes the response to electronic recoils. We provide the response between 0.05 and 10 keV.


s2_response_n_produced_electrons.csv
-------------------------------------

This is the response matrix of our experiment and analysis to electrons released in the liquid. It is a more 'low-level' response matrix, which you can use if you know the spectrum of your dark matter model in terms of number produced electrons. 'Produced' just means 'produced in the liquid xenon': the effects of electron capture on impurities, imperfect extraction etc. are all part of the response matrix. With this matrix, you could (for example) compute our response assuming different charge yield functions.

Columns:
  * `n_produced_electrons`: Number of produced electrons. We provide the response for 0 to 249 produced electrons.
  * `s2_bin_000`: fraction of recoils that produce the number of electrons listed in the first column that cause a signal in the first S2 bin.
  * `s2_bin_001`: similar, for the second S2 bin (bin number 1). 
  * And so forth.


s2_binning_info.csv
-------------------- 

Information on the S2 binning used in the response matrix files.

Columns:
  * `s2_bin_number`: Integer number identifying the S2 bin
  * `start_pe`: (inclusive) start of the bin in photoelectrons (PE)
  * `end_pe`: (exclusve) end of the bin
  * `linear_center_pe`: ordinary average of start_pe and end_pe
  * `log_center_pe`: exp((log(start_pe)+log(end_pe))/2).


events_after_cuts.csv
---------------------

This file contains key observed quantities for the events that pass all S2-only selection cuts (except model-specific S2 region of interest selections). We provide the events with S2 area between 90 and 3000 PE, which are the ones shown in the paper.

Columns:
  * `s2_area_pe`: Integrated area observed for the S2 in photoelectrons. No corrections are applied (e.g. for electron lifetime due to estimated depth). The signal models used to produce the response matrix account for all known relevant detector response effects.
  * `s2_width_ns`: The observed width of the S2 signal in ns. See 'details on the S2 width model' in the paper for details.
  * `has_s1`: True or False, depending on whether the S2 was paired with a valid S1 in the event.


er_and_cenvns_backgrounds.csv
-----------------------------

This contains the number of expected events in each S2 bin of the nominal ER and CEvNS background models. The cathode background is not provided. As we stress several times in the paper, our background model is known to be incomplete, and purposefully conservative.

Columns:
  * `s2_bin_number`: Integer number identifying the S2 bin
  * `er_background_events`: Expected number of events due to the flat energy spectrum electronic recoil background in this S2 bin.
  * `cevns_background_events`: Expected number of events due to solar 8B neutrino coherent neutrino-nucleus scattering in this S2 bin.


