# Skill: API Documentation Standards & Docstring Guidelines

## Description
Instructs developers and code generators on how to write extractable, maintainable API documentation using standardized docstrings. Ensures that inline code comments can be automatically processed to generate comprehensive API reference documentation without additional manual effort.

## Trigger Conditions
- When starting a new Python module or package
- When generating code that will be part of a public API
- When updating code documentation standards for a project
- When setting up documentation generation pipelines (Sphinx, pdoc, etc.)

## Core Philosophy
**Write documentation once, use everywhere:** Developers write docstrings in code; documentation generation tools automatically extract and produce HTML, PDF, and README snippets without duplication.

---

## Docstring Format Standard: Google Style

### Why Google Style for SheetCuts
- Native support in Sphinx with `autodoc_typehints` extension
- Pairs seamlessly with Python type hints (PEP 484+)
- Highly readable in raw source code (minimal markup)
- Industry standard (Google, Uber, AWS SDK)
- Generates cleanest HTML/PDF output
- Enforces consistent structure across team

---

## Mandatory Docstring Sections

### 1. Module-Level Docstring
Every Python file (`.py`) must start with a module docstring.

**Template:**
```python
"""Module name and high-level purpose.

One-paragraph description of what this module does, what problems it solves,
and when a developer should use it. Mention key classes and functions available.

Example:
    Quick example of typical module usage::

        from sheetcuts.optimization import optimize_layout
        layout = optimize_layout(pieces, sheet)

"""
```

### 2. Class Docstring
Every public class must have a docstring describing its purpose, attributes, and usage.

**Template:**
```python
class CuttingPiece:
    """Represents a rectangular piece to be cut from a sheet.
    
    Stores dimensions, quantity, label, and cutting rotation preference.
    Immutable after creation; use builder pattern for complex configurations.
    
    Attributes:
        width (float): Piece width in specified units (inches or mm).
        height (float): Piece height in specified units.
        label (str): Human-readable identifier (e.g., "TopPanel", "P1").
        quantity (int): Number of identical pieces needed. Defaults to 1.
        rotatable (bool): Whether algorithm can rotate piece 90 degrees.
            Defaults to True for homogenous sheets, False for directional.
    
    Raises:
        ValueError: If width or height <= 0.
        TypeError: If label is not a string or quantity is not positive int.
    
    Example:
        >>> piece = CuttingPiece(width=10, height=20, label="Panel", quantity=3)
        >>> print(f"{piece.label} x{piece.quantity}: {piece.width}×{piece.height}")
        Panel x3: 10×20
    """
```

### 3. Function/Method Docstring
Every public function and method must include full documentation.

**Template:**
```python
def optimize_sheet_layout(pieces: list[CuttingPiece], 
                         sheet: Sheet,
                         algorithm: str = "guillotine") -> list[SheetLayout]:
    """Optimizes cutting piece layout on sheets using specified packing algorithm.
    
    Processes list of cutting pieces and automatically arranges them on sheets
    to minimize total sheets used (minimize material waste). Automatically rotates
    pieces 90 degrees if it improves packing on homogenous sheets; respects
    directional constraints on feature-aligned sheets.
    
    Args:
        pieces: List of CuttingPiece objects to be laid out.
        sheet: Sheet specification (dimensions, type, unit system).
        algorithm: Packing algorithm to use. Options: "guillotine" (default),
            "best_area_fit" (future). Defaults to "guillotine".
    
    Returns:
        List of SheetLayout objects, one per sheet required. Each layout contains
        placed pieces with final coordinates (x, y), dimensions, and rotation info.
        List is ordered by sheet usage; empty list if pieces list is empty.
    
    Raises:
        ValueError: If any piece dimension exceeds sheet dimension.
        ValueError: If sheet dimensions are invalid (≤ 0).
        TypeError: If pieces is not a list or sheet is not a Sheet object.
        NotImplementedError: If algorithm specified is not supported.
    
    Performance:
        Time complexity: O(n log n) where n = total number of pieces (including quantities).
        Space complexity: O(n) for output layouts.
        Expected runtime: < 5 seconds for up to 1000 pieces on standard hardware.
    
    Note:
        - Deterministic: same input always produces identical output.
        - Pieces are placed without overlap; all within sheet boundaries.
        - Output coordinates use top-left (0, 0) origin.
    
    Example:
        >>> from sheetcuts.models import CuttingPiece, Sheet
        >>> from sheetcuts.optimization import optimize_sheet_layout
        >>> pieces = [
        ...     CuttingPiece(10, 20, "Panel", quantity=1),
        ...     CuttingPiece(12, 18, "Shelf", quantity=3)
        ... ]
        >>> sheet = Sheet(48, 96, units="inches", sheet_type="homogenous")
        >>> layouts = optimize_sheet_layout(pieces, sheet)
        >>> print(f"Sheets required: {len(layouts)}")
        Sheets required: 1
        >>> for sheet_layout in layouts:
        ...     for placement in sheet_layout.placements:
        ...         print(f"{placement.piece.label}: ({placement.x}, {placement.y})")
        Panel: (0, 0)
        Shelf: (10, 0)
        ...
    """
```

### 4. Property/Attribute Docstring
Document properties and instance attributes.

```python
@property
def utilization_percentage(self) -> float:
    """Percentage of sheet area actually used by cutting pieces.
    
    Calculated as: (total_pieces_area / total_sheet_area) * 100
    
    Returns:
        Float between 0 and 100 representing utilization percentage.
        100.0 means perfect packing with no waste.
    """
    return (self.total_pieces_area / self.sheet_area) * 100
```

---

## Documentation Standards

### Type Hints: MANDATORY
Every function parameter and return value must have type hints.

```python
# ✅ CORRECT
def validate_piece(piece: CuttingPiece) -> bool:
    """Check if piece is valid for processing."""

# ❌ INCORRECT (no type hints)
def validate_piece(piece):
    """Check if piece is valid for processing."""

# ✅ ALSO CORRECT (complex types)
def process_pieces(pieces: list[CuttingPiece] | tuple[CuttingPiece, ...]) -> dict[str, SheetLayout]:
    """Process multiple piece collections."""
```

### Docstring Content Rules
1. **First line:** One-sentence summary (should fit on one line, < 80 chars)
2. **Blank line:** Always separate summary from detailed description
3. **Args section:** Use `name (type): Description` format
4. **Returns section:** Describe return value, its type, and what it means
5. **Raises section:** List all exceptions that function can raise
6. **Example section:** Include realistic, runnable code examples
7. **Note section:** Add non-obvious implementation details, performance warnings, or gotchas

### Formatting Rules
- Use 4-space indentation for continuation lines
- Indent code blocks with 8+ spaces or use `::` prefix and blank line
- Use backticks for code references: `variable_name`, `ClassName`, `function()`
- Use **bold** for emphasis: `**important**`
- Line length: Keep ≤ 79 chars where possible (compatible with various viewers)

### Example Section Format
```python
    Example:
        Runnable code demonstrating the function::

            from mymodule import MyClass
            obj = MyClass(param1=10)
            result = obj.method()
            print(result)

        Multiple examples are acceptable for complex functions::

            # Example 1: Simple case
            simple_result = function(simple_input)

            # Example 2: Complex case with error handling
            try:
                complex_result = function(complex_input)
            except ValueError as e:
                print(f"Invalid input: {e}")
```

---

## Module Structure for API Documentation

```
sheetcuts/
├── __init__.py              # Main module docstring; expose public API
├── models.py                # Classes: CuttingPiece, Sheet, SheetLayout, etc.
├── optimization.py          # Core functions: optimize_sheet_layout(), etc.
├── validators.py            # Validation functions (public if external validation needed)
├── visualization.py         # SVG/HTML generation (public: generate_html_diagram())
└── _internal/               # Private implementation (underscore prefix)
    ├── guillotine.py        # Internal algorithm implementation
    └── geometry.py          # Internal geometry helpers
```

**Public API (documented in generated docs):**
- `models.py` - All classes
- `optimization.py` - All functions (main entry points)
- `visualization.py` - `generate_html_diagram()`
- Top-level functions re-exported in `__init__.py`

**Private/Internal (excluded from generated docs):**
- Any function/class with `_` prefix
- `_internal/` module contents

---

## Documentation Generation Workflow

### Step 1: Configure Sphinx (setup done once)
```bash
# Installed via setup.py or requirements-dev.txt
pip install sphinx sphinx-rtd-theme sphinx-autodoc-typehints
```

### Step 2: Sphinx Configuration (conf.py)
```python
extensions = [
    'sphinx.ext.autodoc',           # Auto-extract docstrings
    'sphinx.ext.autodoc.typehints', # Include type hints
    'sphinx.ext.napoleon',           # Parse Google-style docstrings
    'sphinx.ext.viewcode',           # Link to source code
]
autodoc_typehints = "description"   # Show hints in descriptions
```

### Step 3: Build HTML Documentation
```bash
cd docs/
make html
# Output: _build/html/index.html
```

### Step 4: Extract for README
```bash
# After building, copy key API sections from _build/html to README.md
# Or use Sphinx markdown output for README embedding
```

---

## Documentation Checklist for Developers

- [ ] Module-level docstring present and describes purpose
- [ ] All public classes have docstrings with Attributes and Examples
- [ ] All public functions have complete Args, Returns, Raises sections
- [ ] All parameters and return values have type hints
- [ ] At least one Example section per class/function
- [ ] Complex functions include Performance note
- [ ] Edge cases mentioned in Note section
- [ ] No markup errors (backticks, bold, code blocks render correctly)
- [ ] Docstring length ≤ 79 chars per line (where practical)
- [ ] Examples are realistic and runnable
- [ ] Sphinx builds without warnings: `sphinx-build -W -b html docs/source docs/build`

---

## Integration with CI/CD

Add to pre-commit hooks and CI:
```bash
# Check for missing docstrings
pydocstyle sheetcuts/

# Validate docstring syntax
sphinx-build -W -b html docs/source docs/build
```

---

## Common Mistakes to Avoid

| Mistake | ❌ Wrong | ✅ Correct |
| :--- | :--- | :--- |
| No type hints | `def func(x):` | `def func(x: int) -> str:` |
| Type in description only | `"""Returns the count (int)"""` | `"""Returns: int: The count."""` + type hint |
| Multi-line summary | `"""This is a long\nmulti-line summary"""` | `"""One-line summary.\n\nDetailed description here."""` |
| Inconsistent format | Mix of styles | Google style consistently |
| No Examples | Description only | Always include Example |
| Vague descriptions | `"""Process data"""` | `"""Optimize piece layout on sheets using Guillotine algorithm."""` |
| Missing edge cases | No Note on limits | Note special behaviors, performance, constraints |

---

## Benefits

1. **Single Source of Truth:** Documentation lives in code; stays in sync automatically
2. **Reduced Effort:** Write once, generate everywhere (HTML, PDF, README, IDE tooltips)
3. **IDE Integration:** Modern IDEs parse docstrings; developers see hints on hover
4. **Consistency:** Enforced format ensures professional documentation quality
5. **Traceability:** Every function/class is documented before shipping
6. **Maintainability:** Future developers understand intent without separate wiki

---

## References

- [Google Python Style Guide - Docstrings](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings)
- [Napoleon Extension (Sphinx)](https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html)
- [PEP 257 - Docstring Conventions](https://peps.python.org/pep-0257/)
- [Type Hints (PEP 484)](https://peps.python.org/pep-0484/)
