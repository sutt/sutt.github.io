# Beware pseudo-duplicates over inflating your validation accuracy...
    
![_config.yml]({{ site.baseurl }}/images/pic1.jpg)

cool graphic: Raven's Progressive Matrix of the Queen-King image 10x and then a question mark on the eleveneth. Maybe add a second row of white knights too?

tldr section - near duplicate clusters of images in the coomputer vision datasets are difficult to avoid, difficult to diagnosis and lead to models mis-judging their out of sample generalizability.

## Problem statement: 
you want to measure your model perf, just make a truly random validation set...what could go wrong?

- most famously this is similiar to slicing the end p percent off a time series; interpolation vs extrapolation. racheal thomas diagram

- this represent a subclass of predictive leakage, (like Caludia Perlich) 

- another form of predictive leakage is background (e.g. cats vs dogs)
but one that can't be controlled with feature selection; especially pernicious

### Now introduce the problem of duplicates:

- it leads to memorization and will not generalize well
- but more importatnly it will give you a false sense of having a good model
    - especially pseudo duplicates of n > 1/valid_pct

- walk through the chess example

- psuedo b/c they are almost the same which makes the problem more difficult, can't just do drop_duplicates, which make diagnosing them harder which we discuss in next section. TP are over represented and FN are under-represnted.

### Focus on Generalizability:
        
- this will become apparent when you bring in a true test set, and underscores the importance therof. Since an independantly generated test set is the most definitive signal of this possible anti-patternit  can be sensibly mis-diagnosed as camera / lighting artifacts. Notice the trend of time wasting thief.

- Notice there's no naive splitting function that can generate a validation set that truly measures out-of-sample performance, and there isn't nec. a way straight-forward way to do EDA/pre-processing to split either; notice how if you allocated full images to train/valid and then took the crops you'd still have the problem, because certain images are pseduo-duplicates of each other. Cross validation and all the seeds are unlikely to reveal your error.

### how to diagnose? 

- eyeball: take it from me, it's not easy to eyeballs cluster k > 3 when doing it on unsorted cropped images. This can be common for many tasks like street signs. But once you've got them clustered we can easily verify by eye.

- it's difficult because pixel shifts, etc make comparisons fuzzy, but we can't just do some aggregate distance either because in-label statistics should be similar in many to each other and different from each other.

- one difference in situation is when you have access to the meta-data nad logs associated with the data generation (or control it) or are ignorant to it.
    - you might be able to exploit the desing of the experiment to eliminate pseduo-duplicates
    - see siRNA challenge of Giulia

- you can slide the validation split in the 69-90% range and see what happens on different seeds. maybe doing k-fold on validation set?

- what kind of situations might generate pseduo-duplicates

    - same camera-angle+pose * diff-lighting samples

    - occur in repetively, articifically generated samples

    - frame-like: 1 turn of a game at a time.

    - most generally: shared background, or neighboring classes, same pose

    - how to avoid generating them, or keep track of them
