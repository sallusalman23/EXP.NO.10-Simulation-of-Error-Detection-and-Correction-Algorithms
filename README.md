# EXP.NO.10-Simulation-of-Error-Detection-and-Correction-Algorithms

# AIM

To implement and simulate a linear block code using a generator matrix and parity-check matrix to generate codewords, detect errors, and correct single-bit errors using syndrome decoding.

# SOFTWARE REQUIRED
Python (preferably 3.x)

Python packages: NumPy

# ALGORITHMS
```
Input the number of message bits and parity bits.

Enter the parity matrix 
𝑃
P.

Form the generator matrix 
𝐺
=
[
𝐼
∣
𝑃
]
G=[I∣P].

Generate all possible message bit combinations.

Multiply each message with 
𝐺
G to get the codewords.

Calculate Hamming weight of each codeword.

Find the minimum Hamming distance.

Form the parity check matrix 
𝐻
=
[
𝑃
𝑇
∣
𝐼
]
H=[P 
T
 ∣I].

Input a received codeword.

Calculate the syndrome 
𝑠
=
𝑟
⋅
𝐻
𝑇
m
o
d
 
 
2
s=r⋅H 
T
 mod2.

Compare the syndrome with columns of 
𝐻
𝑇
H 
T
  to find the error position.

Correct the error by flipping the corresponding bit.

Display the corrected codeword and all results.

```

# PROGRAM
```
import numpy as np

pb = \[]           # Parity matrix rows
Ik = \[]           # Identity matrix
p = \[]
m = \[]
h\_dis = \[]
r\_code = \[]
err = \[]

# Input for matrix dimensions

col = int(input("Enter the number of parity bits: "))
row = int(input("Enter the number of message bits: "))

# Input the parity matrix (P)

print("\nEnter the parity matrix rows:")
for i in range(row):
p = list(map(int, input(f"Row {i+1}: ").split()))
if len(p) != col:
raise ValueError(f"Each row must have {col} elements.")
pb.append(p)

# Convert to numpy array

p\_mat = np.array(pb, dtype=int)
Ik = np.eye(row, dtype=int)

# Generator Matrix G = \[I | P]

g\_mat = np.hstack((Ik, p\_mat))

# Codeword length n and message bits k

k = g\_mat.shape\[0]
n = g\_mat.shape\[1]

# Generate all possible message vectors (2^k)

m = np.array(\[\[1 if (i >> (k - j - 1)) & 1 else 0 for j in range(k)] for i in range(2 \*\* k)])

# Generate codewords: c = m \* G mod 2

c = np.mod(np.dot(m, g\_mat), 2)

# Hamming weights

for i, row in enumerate(c):
h\_dis1 = np.sum(row)
h\_dis.append(h\_dis1)
h\_mat = np.array(h\_dis).reshape(-1, 1)
d\_min = np.min(h\_dis\[1:])

# Parity-check matrix H = \[P^T | I]

p\_t = p\_mat.T
h\_check = np.hstack((p\_t, np.eye(col, dtype=int)))
ht = h\_check.T  # Transpose of H

# Output Generator Matrix

print("\n\*\*\*\*\*\*\*\*\*\*")
print("Generator Matrix \[G = I | P]:")
for row in g\_mat:
print(" ".join(map(str, row)))

# Output Codewords

print("\n\*\*\*\*\*\*\*\*\*\*")
print("Message Bits\tCodeword\tHamming Weight")
for i in range(len(m)):
msg\_str = " ".join(map(str, m\[i]))
code\_str = " ".join(map(str, c\[i]))
print(f"{msg\_str}\t{code\_str}\t\t{h\_dis\[i]}")

# Minimum Hamming distance

print("\n\*\*\*\*\*\*\*\*\*\*")
print(f"Minimum Hamming Distance: {d\_min}")

# Output Parity Check Matrix

print("\n\*\*\*\*\*\*\*\*\*\*")
print("Parity Check Matrix \[H = P^T | I]:")
for row in h\_check:
print(" ".join(map(str, row)))

# Output Transpose of Parity Check Matrix

print("\n\*\*\*\*\*\*\*\*\*\*")
print("Transpose of Parity Check Matrix (H^T):")
for row in ht:
print(" ".join(map(str, row)))

# Receive codeword

rc = list(map(int, input("\nEnter the received codeword: ").split()))
if len(rc) != n:
raise ValueError("Received codeword length must match codeword length n.")
r\_c = np.array(\[rc])

# Syndrome calculation: s = r \* H^T mod 2

e = np.mod(np.dot(r\_c, ht), 2).flatten()

# Find error position

err = np.zeros(n, dtype=int)
for i in range(n):
if np.array\_equal(e, ht\[i]):
err\[i] = 1
break

print("\n\*\*\*\*\*\*\*\*\*\*")
print("Syndrome:", " ".join(map(str, e)))
print("Error vector:", " ".join(map(str, err)))

# Correct the error

corrected = (r\_c.flatten() + err) % 2
print("Corrected Codeword:", " ".join(map(str, corrected)))

# Optional: Syndrome Table (first few entries)

print("\n\*\*\*\*\*\*\*\*\*\*")
print("Syndrome Matrix:")
for i in range(n):
s = ht\[i]
ev = np.eye(n, dtype=int)\[i]
print(f"{' '.join(map(str, s))}  {' '.join(map(str, ev))}")

print("\*\*\*\*\*\*\*\*\*\*")
```

# OUTPUT
![image](https://github.com/user-attachments/assets/e4d47512-155a-4e59-adf3-90328647b8f4)
![image](https://github.com/user-attachments/assets/6b776bbe-5609-494a-89cc-5186fd1f5d4d)


 
# RESULT / CONCLUSIONS

The simulation:

Generates all possible codewords using the generator matrix.

Computes the minimum Hamming distance.

Constructs the parity-check matrix and its transpose.

Accepts a received codeword, computes its syndrome, and identifies and corrects single-bit errors.

Validates the error-correcting capability of the code.


