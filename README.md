PACKAGE DOCUMENTATION

package bit
    import "go-bit"

    Package bit provides a bitset implementation and utility bit functions
    for uint64 words.


CONSTANTS

const (
    MaxInt  = 1<<(BitsPerWord-1) - 1 // either 1<<31 - 1 or 1<<63 - 1
    MinInt  = -MaxInt - 1            // either -1 << 31 or -1 << 63
    MaxUint = 1<<BitsPerWord - 1     // either 1<<32 - 1 or 1<<64 - 1
)
    Implementation-specific integer limit values.

const BitsPerWord = bitsPerWord // either 32 or 64

    Implementation-specific size of int and uint in bits.


FUNCTIONS

func Count(w uint64) int
    Count returns the number of nonzero bits in w.

func MaxPos(w uint64) int
    MaxPos returns the position of the maximum nonzero bit in w, w ≠ 0. It
    panics for w = 0.

func MinPos(w uint64) int
    MinPos returns the position of the minimum nonzero bit in w, w ≠ 0. It
    panics for w = 0.


TYPES

type Set struct {
    // contains filtered or unexported fields
}
    Set represents a mutable set of non-negative integers. The zero value of
    Set is an empty set ready to use. A set occupies approximately n bits,
    where n is the maximum value that has been stored in the set.


func New(n ...int) (S *Set)
    New creates a new set S with the given elements.


func (S *Set) Add(n int) *Set
    Add adds n to S, setting S to S ∪ {n}, and returns the updated set.

func (S *Set) AddRange(m, n int) *Set
    AddRange adds all integers between m and n-1, m ≤ n, setting S to S ∪
    {m..n-1}, and returns the updated set.

func (A *Set) And(B *Set) (S *Set)
    And creates a new intersection S = A ∩ B that consists of all elements
    that belong to both A and B.

func (A *Set) AndNot(B *Set) (S *Set)
    AndNot creates a new set difference S = A ∖ B that consists of all
    elements that belong to A, but not to B.

func (S *Set) Clear() *Set
    Clear removes all elements and returns the updated empty set.

func (S *Set) Contains(n int) bool
    Contains returns true if n, n ≥ 0, is an element of S.

func (S *Set) Do(f func(n int))
    Do calls function f for each element n ∊ S in numerical order. It is
    safe for f to add or remove elements e, e ≤ n, from S. The behavior of
    Do is undefined if f changes the set in any other way.

func (A *Set) Equals(B *Set) bool
    Equals returns true if A and B contain the same elements.

func (S *Set) Flip(n int) *Set
    Flip removes n from S if it is present, otherwise it adds n, setting S
    to S ∆ {n}, and returns the updated set.

func (S *Set) FlipRange(m, n int) *Set
    FlipRange flips all elements in the range m..n-1, m ≤ n, setting S to S
    ∆ {m..n-1}, and returns the updated set.

func (A *Set) Intersects(B *Set) bool
    Intersects returns true if A and B overlap, i.e. A ∩ B ≠ ∅.

func (S *Set) IsEmpty() bool
    IsEmpty returns true if S = ∅.

func (S *Set) Max() int
    Max returns the maximum element of the set. It panics if S is empty.

func (S *Set) Min() int
    Min returns the minimum element of the set. It panics if S is empty.

func (S *Set) Next(m int) (n int, found bool)
    Next returns (n, true), where n is the smallest element of S such that m
    < n, or (0, false) if no such element exists.

func (A *Set) Or(B *Set) (S *Set)
    Or creates a new union S = A ∪ B that contains all elements that belong
    to either A or B.

func (S *Set) Previous(m int) (n int, found bool)
    Previous returns (n, true), where n is the largest element of S such
    that n < m, or (0, false) if no such element exists.

func (S *Set) Remove(n int) *Set
    Remove removes n from S, setting S to S ∖ {n}, and returns the updated
    set.

func (S *Set) RemoveMax() (max int)
    RemoveMax removes the maximum element from S, setting S to S ∖ {max},
    and returns max. It panics if S is empty.

func (S *Set) RemoveMin() (min int)
    RemoveMin removes the minimum element from S, setting S to S ∖ {min},
    and returns min. It panics if S is empty.

func (S *Set) RemoveRange(m, n int) *Set
    RemoveRange removes all integers between m and n-1, m ≤ n, setting S to
    S ∖ {m..n-1}, and returns the updated set.

func (S *Set) Set(A *Set) *Set
    Set sets S to A and returns S.

func (S *Set) SetAnd(A, B *Set) *Set
    SetAnd sets S to the intersection A ∩ B and returns S.

func (S *Set) SetAndNot(A, B *Set) *Set
    SetAndNot sets S to the set difference A ∖ B and returns S.

func (S *Set) SetOr(A, B *Set) *Set
    SetOr sets S to the union A ∪ B and returns S.

func (S *Set) SetWord(i int, w uint64) *Set
    SetWord interprets w as a bitset with numbers in the range 64i to 64i +
    63, where 0 ≤ i ≤ ⌊MaxInt/64⌋, overwrites this range in S with w, and
    returns S.

func (S *Set) SetXor(A, B *Set) *Set
    SetXor sets S to the symmetric difference A ∆ B = (A ∪ B) ∖ (A ∩ B) and
    returns S.

func (S *Set) Size() int
    Size returns |S|, the number of elements in S. This method scans the
    set. To check if a set is empty, consider using the more efficient
    IsEmpty.

func (S *Set) String() string
    String returns a string representation of S. The elements are listed in
    ascending order, enclosed by braces, and separated by ", ". Runs of at
    least three elements a, a+1, ..., b are given as "a..b".

    For example, the set {1, 2, 6, 5, 3} is represented as "{1..3, 5, 6}".

func (A *Set) SubsetOf(B *Set) bool
    SubsetOf returns true if A ⊆ B.

func (S *Set) Word(i int) (w uint64)
    Word returns the range 64i to 64i + 63 of S as a bitset represented by
    w.

func (A *Set) Xor(B *Set) (S *Set)
    Xor creates a new symmetric difference S = A ∆ B = (A ∪ B) ∖ (A ∩ B)
    that consists of all elements that belong to either A or B, but not to
    both.



