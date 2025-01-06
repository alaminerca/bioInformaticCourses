# Bioinformatics Learning Module

## Basic DNA Sequence Analysis

```python
class DNAToolkit:
    """Basic DNA sequence analysis tools"""
    
    def __init__(self, sequence):
        """Initialize with DNA sequence"""
        self.sequence = sequence.upper()
        self.valid_bases = set('ATCG')
        
    def validate_sequence(self):
        """Check if sequence contains only valid DNA bases"""
        return all(base in self.valid_bases for base in self.sequence)
    
    def sequence_length(self):
        """Return sequence length"""
        return len(self.sequence)
    
    def nucleotide_count(self):
        """Count occurrences of each nucleotide"""
        return {
            'A': self.sequence.count('A'),
            'T': self.sequence.count('T'),
            'G': self.sequence.count('G'),
            'C': self.sequence.count('C')
        }
    
    def gc_content(self):
        """Calculate GC content percentage"""
        counts = self.nucleotide_count()
        gc = counts['G'] + counts['C']
        total = sum(counts.values())
        return (gc / total) * 100 if total > 0 else 0
    
    def find_start_codons(self):
        """Find all start codon (ATG) positions"""
        positions = []
        for i in range(0, len(self.sequence) - 2):
            if self.sequence[i:i+3] == 'ATG':
                positions.append(i + 1)  # 1-based position
        return positions
    
    def find_stop_codons(self):
        """Find all stop codon (TAA, TAG, TGA) positions"""
        stop_codons = ['TAA', 'TAG', 'TGA']
        positions = []
        for i in range(0, len(self.sequence) - 2):
            if self.sequence[i:i+3] in stop_codons:
                positions.append(i + 1)  # 1-based position
        return positions
    
    def reverse_complement(self):
        """Generate reverse complement of sequence"""
        complement_dict = {'A': 'T', 'T': 'A', 'G': 'C', 'C': 'G'}
        complement = ''.join(complement_dict[base] for base in self.sequence)
        return complement[::-1]

```

## FASTA File Analysis (Using BioPython)

```python
from Bio import SeqIO

class FastaAnalyzer:
    """FASTA file analysis tools"""
    
    def __init__(self, fasta_file):
        """Initialize with FASTA file path"""
        self.fasta_file = fasta_file
        self.records = list(SeqIO.parse(fasta_file, "fasta"))
        
    def count_records(self):
        """Count number of records in FASTA file"""
        return len(self.records)
    
    def get_sequence_lengths(self):
        """Get dictionary of sequence IDs and their lengths"""
        return {record.id: len(record.seq) for record in self.records}
    
    def find_extreme_sequences(self):
        """Find longest and shortest sequences"""
        if not self.records:
            return None
            
        lengths = self.get_sequence_lengths()
        min_len = min(lengths.values())
        max_len = max(lengths.values())
        
        shortest = [id for id, length in lengths.items() if length == min_len]
        longest = [id for id, length in lengths.items() if length == max_len]
        
        return {
            'shortest': {'length': min_len, 'ids': shortest},
            'longest': {'length': max_len, 'ids': longest}
        }
```

## ORF Finding

```python
class ORFFinder:
    """Open Reading Frame analysis tools"""
    
    def __init__(self, sequence):
        """Initialize with DNA sequence"""
        self.sequence = sequence.upper()
        
    def get_reading_frame(self, frame):
        """Get sequence in specified reading frame (1, 2, or 3)"""
        return self.sequence[frame-1:]
    
    def find_orfs_in_frame(self, frame):
        """Find all ORFs in specified reading frame"""
        seq = self.get_reading_frame(frame)
        orfs = []
        
        i = 0
        while i < len(seq) - 5:  # Minimum ORF length is 6 (start + stop)
            # Look for start codon
            if seq[i:i+3] == 'ATG':
                # Look for stop codon
                j = i + 3
                while j < len(seq) - 2:
                    codon = seq[j:j+3]
                    if codon in ['TAA', 'TAG', 'TGA']:
                        orfs.append({
                            'start': i + frame,  # 1-based position
                            'end': j + frame + 2,
                            'sequence': seq[i:j+3],
                            'length': j + 3 - i
                        })
                        break
                    j += 3
            i += 3
        return orfs
    
    def find_all_orfs(self):
        """Find ORFs in all three forward frames"""
        all_orfs = []
        for frame in [1, 2, 3]:
            all_orfs.extend(self.find_orfs_in_frame(frame))
        return all_orfs
    
    def get_longest_orf(self):
        """Find the longest ORF across all frames"""
        orfs = self.find_all_orfs()
        if not orfs:
            return None
        return max(orfs, key=lambda x: x['length'])
```

## DNA Repeat Finder

```python
class RepeatFinder:
    """DNA repeat sequence analysis tools"""
    
    def __init__(self, sequence):
        """Initialize with DNA sequence"""
        self.sequence = sequence.upper()
        
    def find_repeats(self, length):
        """Find all repeats of specified length"""
        repeats = {}
        
        # Generate all possible substrings of given length
        for i in range(len(self.sequence) - length + 1):
            substring = self.sequence[i:i+length]
            count = 0
            
            # Count occurrences of substring
            for j in range(len(self.sequence) - length + 1):
                if self.sequence[j:j+length] == substring:
                    count += 1
                    
            # Store if it's a repeat (occurs more than once)
            if count > 1:
                repeats[substring] = count
                
        return repeats
    
    def most_frequent_repeat(self, length):
        """Find most frequent repeat of specified length"""
        repeats = self.find_repeats(length)
        if not repeats:
            return None
        
        max_count = max(repeats.values())
        most_frequent = [seq for seq, count in repeats.items() 
                        if count == max_count]
        
        return {
            'sequences': most_frequent,
            'count': max_count
        }
```

## Example Usage

```python
# Example DNA sequence analysis
dna_seq = "ATGGCCTAAGGCTGATAGATGGCCTAA"
toolkit = DNAToolkit(dna_seq)

print(f"Sequence Length: {toolkit.sequence_length()}")
print(f"Nucleotide Counts: {toolkit.nucleotide_count()}")
print(f"GC Content: {toolkit.gc_content():.2f}%")
print(f"Start Codons at: {toolkit.find_start_codons()}")
print(f"Stop Codons at: {toolkit.find_stop_codons()}")
print(f"Reverse Complement: {toolkit.reverse_complement()}")

# Example FASTA analysis
fasta_analyzer = FastaAnalyzer("example.fasta")
print(f"Number of Records: {fasta_analyzer.count_records()}")
print(f"Sequence Lengths: {fasta_analyzer.get_sequence_lengths()}")
print(f"Extreme Sequences: {fasta_analyzer.find_extreme_sequences()}")

# Example ORF finding
orf_finder = ORFFinder(dna_seq)
print(f"Longest ORF: {orf_finder.get_longest_orf()}")

# Example repeat finding
repeat_finder = RepeatFinder(dna_seq)
print(f"Repeats of length 3: {repeat_finder.find_repeats(3)}")
print(f"Most frequent repeat: {repeat_finder.most_frequent_repeat(3)}")
```

This code provides:

1. Clear class-based organization for each analysis type
2. Detailed documentation for all functions
3. Simple implementations focused on learning
4. Example usage for each feature
5. Easy to extend or modify functionality

Each class can be used independently or combined for more complex analyses. The code prioritizes readability and understanding over optimization.

To use in a notebook:
1. Copy each class definition to a separate cell
2. Run example code in separate cells
3. Modify input sequences/parameters as needed

For larger datasets, consider:
1. Adding file handling for sequence input
2. Implementing batch processing
3. Adding error handling for invalid inputs
4. Optimizing repeat finding for longer sequences