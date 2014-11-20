Downloaded data from DRYAD on 2014-11-14 at 9:00:35AM
		wget http://datadryad.org/bitstream/handle/10255/dryad.72024/README.txt
		wget http://datadryad.org/bitstream/handle/10255/dryad.72024/data%20Barrett%20et%20al%20JEcol%202014.csv

renamed the csv to `Barrett2014.csv`, and `README.txt` to `metadata.txt`

I made a csv that added more variables based on the bact.trt in Barrett2014.csv


rhiz.strain = strain ID from their `metadata.txt`
rhiz.genus = the genus/genera of the inoculant, and mix where they are mixed
bact.dv = the number of different genera present

## Variables in the processed data

- `Plant` is which plant in the pot it was, A or B (two plants per pot)
- `Wt` is the plant dry weight in grams
- `Block` is the block that the plants were grown in
- `host.trt` is the species of *Acacia* that were in the pot, *A.salicina*, *A. stenophylla* or *A.salicina* & *A. stenophlla*
- `species` is the species of the individual plant
- `n.strains` - the number of different rhizobia strains that were present
- `nodulation` - the nodulation score, "a categorical assessment of nodule biomass based on number and size (ranging from 1 to 5: 1 = small number (< 10) of small nodules (~1–2 mm in diameter); 5 = numerous large N2 fixing nodules with pink/red centres)" -- *Note: not really sure how this was determined - Derek*
- `rhiz.strain` - the rhizobial strain used in the treatment
- `rhiz.genus` - the genera present, brady = *Bradyrhizobium*, meso = *Mesorhizobium*, rhizo = *Rhizobium*, sino = *Sinorhizobium*. 
- `bact.div` - the number of different bacterial genera present

