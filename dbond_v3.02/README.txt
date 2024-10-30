/*************************************************************************/
/*                                                                       */
/*      DBond v3.02  : Identification of Intact Disulfide Linkages       */
/*                     Release: October 1, 2012                          */
/*                  Hanyang University, Seoul, Korea                     */
/*                                                                       */
/*   New Algorithm for the Identification of Intact Disulfide Linkages   */
/*     Based on Fragmentation Characteristics in Tandem Mass Spectra     */
/*                                                                       */
/*************************************************************************/

1. Requirement
DBond requires Java 1.6.0_31 or later. Visit http://www.oracle.com/technetwork/java/index.html 


2. Usage
Command  : java -jar DBond.jar <options> <file>
Options  :
  -i <input_filename> : Config file path for search parameters [Required]
  -o <output_filename>: Output file path for search results [Optional]
                      : If not specified, it is automatically generated (*.bond.txt)

Example1 : java -jar DBond.jar -i foo.txt -o out.txt
Example2 : java -jar DBond.jar -i foo.txt


3. Searching
To run a search, a DBond config file has to be given as text file.
Each line of the file has the form of parameter=[VALUE].

 Example)
   Spectra= foo.mgf
   Fasta= foo.fasta
   PeptTolerance= 0.5
   Mod= Oxidation, M, M, 15.994915



Spectra=[FILENAME] : Specifies path to the spectra file to search. 
                     Supported formats are *.mgf, *.pkl and *.dta.
                     In case of dta type, for multiple MS/MS scans, 
                     at least one blank line must be between each MS/MS scan.

Instrument=[TYPE] : Specifies the type of MS/MS instrument used (or best matched) for peptide fragmentation.
                    According to the instrument type, a different fragmentation model is applied.
                    TYPE={ESI-TRAP|ESI-QTOF}
                    Default value is ESI-TRAP.

Fasta=[FILENAME] : Specifies path to the database file (*.fasta) to search

PeptTolerance=[MASS] : Sets a parent mass tolerance in dalton. 
                       Default value is 1.2.

PPMtolerance=[MASS] : Sets a parent mass tolerance in ppm

FragTolerance=[MASS] : Sets a fragment ion mass tolerance in dalton. 
                       Default value is 0.6.

Enzyme=[NAME]{,[CLEAVAGE/TERMINUS]}* : Specifies the reagent used for protein digestion. 
                                       CLEAVAGE=residues specific to the enzyme. Serveral residues can be concatenated.
				       TERMINUS={N|C}, side of the residues the enzyme cleaves at. 'N'=amino side, 'C'=carboxyl side.
				       Default value is 'Trypsin, KR/C'.
				       Users are free to define an enzyme. Ex) 'myenzyme, D/N, D/C, FL/C'.
				       Separating multiple cleavage rules with a comma can be added. 
				       This enables the use of multiple enzymes in searching. Ex) 'Trypsin+GluC+LysN, KR/C, E/C, K/N'
				       Use 'NONE, */*' to ignore the enzyme specificity (for non-enzyme search).

MissedCleavage=[NUMBER] : Sets the number of allowed missed cleavage sites. 
                          Default value is 1.

{Mod=[NAME],[POSITION],[RESIDUE],[MASS]}* : Specifies post-translational modifications.
                                            POSITION={Nterm|Cterm|amino acids}
                                            RESIDUE={Nterm|Cterm|amino acids}
                                            MASS in dalton                                            
                                            To specify multiple modifications, add new line with new modification. 
					    Multiple lines are allowed only for lines starting with "Mod=".
					    For a guide to various known modification types, consult http://www.unimod.org.
 
FreeCysteineBlock=[NAME],[MASS]: Specifies the chemical derivative to a free (non-disulfide bond) cysteine by the reaction, 
                                 if free cysteines were treated to minimize/block the disulfide bond interchange.							

Interaction=[FLAG]: Allows disulfide bond linkage between different proteins. 
                    FLAG={true|false}
                    Default value is false.

4. Output (tab-separated)
>>spectrum_name	 observed_MW  charge_state
rank  calculated_MW  delta_mass  score  peptideA  bond_siteA  proteinA  peptideB  bond_siteB  proteinB
- spectrum_name   : spectrum name
- observed_MW     : molecular weight of the precursor ion
- charge_state    : charge state of the precursor ion
- rank            : rank
- calculated_MW   : molecular weight of the disulfied-linked peptide 
- delta_mass      : observed_MW minus calculated_MW
- score           : score of the peptide-spectrum match (normalized intensity sum of matched fragment ions)
                  : The threshold of 20(for QTOF) ~ 30(for IONTRAP) can be normally used to select ID. 
                  : Because the score tends to increase with the molecular weight, the care can be required in big molecular weights (>4000).
- peptideA        : one peptide A of the linked peptides
- bond_siteA      : Cys position for the disulfide within protien A
- proteinA        : protein of peptide A
- peptideB        : the other peptide B of the linked peptides
- bond_siteB      : Cys position for the disulfide within protien B
- proteinB        : protein of peptide B

5. DBond Viewer
To help users inspect MS/MS spectra manually, a spectral viewer is provided. 
The input XML file to a viewer can be obtained from search results. (*.bond.xml)
For the usage, visit http://prix.hanyang.ac.kr/download/dbond.jsp

6. Citation
If you want to refer to DBond software, please cite
Choi, S., Jeong, J., Na, S., Lee, H. S., Kim, H. Y., Lee, K. J., and Paek, E. 
New algorithm for the identification of intact disulfide linkages based on fragmentation characteristics in tandem mass spectra.
Journal of Proteome Research, 2010, 9, 626-635.

7. Lisence
DBond is licensed only for academic research purposes. It is not permitted 
to distribute this program to other users. We grant a personal, non-exclusive, 
non-transferable, royalty-free and indivisible license.

8. Contact
For feedback, questions and comments, contact us at prix@hanyang.ac.kr.



