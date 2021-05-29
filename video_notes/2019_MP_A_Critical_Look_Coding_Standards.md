# Notes from CppCon 2019: Michael Price “A Critical Look at the Coding Standards Landscape”

Some Common Guidelines
1. C++ Core Guidelines
2. AUTOSAR C++14 Guidelines
3. SEI CERT C++ Coding Standard
4. Jason Turner's C++ Best Practices
5. "Effective Modern C++", S. Meyers

## WHy have guidelines ?
A starting point to not to think of details and still maintain a basic level of quality.
Things that we are trying to achieve :

1. Clarity
2. Future Proofing
3. Meet Constrining Factors(limited amount of memory or time)
4. Avoid Undefined Behavior
5. Portability
6. Interaction with Tooling
7. You want to eliminate rediculous arguments

## Techniques

1. Formatting (Guidelines that specify the way that code looks, Braces, Whitespace, line width).
2. Organization (How elements of code are organized, namespace depth, function length, private/public headers, single class source files, class ccessibility order)
3. Naming (How files and elements in code are named, Hungarian Notation. Library-specific Prefixes, Casing)
4. Explicitness (Guidelines that require or refer behabior to be explicitly written in situations where it otherwise might not be required, no auto locals, rule of five, virtual/override keyword usage)
5. Feature Restriction (example no non-const format strings, no iostreams, no dynamic memory allocation, no exception)
6. Feature Promotion ( almost always auto, use concepts)
7. Behavioral Prescriptions ( use source contro, enable more warnings, treat warnings as errors, write tests, compile with multiple compilers, use analysis tools, the best bet is the static analyzer that you can run as part of your automated build system.)

### "All changes have a non-zero probability of introducing a fault. So when we try to adhere to a guideline in a codebase that is already we may inadvertantly introduce a bug at a place that was working fine before." - Approximate quote from Cathal Boogerd and Leon Moonen, Assessing the Value of Coding Standards: An Empirical Study. August 23, 2009

## Productivity

Strictly adhering to some guidelines can cause significant amount of time to be spent understanding violations and addressing them, with a questionable return on investment.

Could that time be better spent doing something with a higher impact towards achieving goals ?

## Some quotes

    Guidelines are not a replacement for a code review process
    Guidelines are not a replacement for automated testing
    Use tools to automate the guidelines that don't take much thought.

## Ending notes

1. Guidelines should avoid inventing terminoloty that are not in the standard. Example "same".
2. Define a process to regularly review and update your guidelines. Perform that process religiously

[HOME](../README.md)