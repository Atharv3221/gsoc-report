# Google Summer of Code 2026 Final Report

### **Project**: [Improve Google Style coverage on false negatives](https://github.com/checkstyle/checkstyle/wiki/Checkstyle-GSoC-2026-Project-Ideas#project-name-improve-google-style-coverage-on-false-negatives)

### **Student**: [Atharv Chavan](https://github.com/Atharv3221)

### **Organization**: [Checkstyle](https://github.com/checkstyle)

### **Mentors**: [Roman Ivanov](https://github.com/romani), [Mauryan Kansara](https://github.com/Zopsss), [Mohit Sharma](https://github.com/mohitsatr)

---

### Project Goals

The project aims to resolve false negatives for [google_checks.xml](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml). The [google_checks.xml](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml) is the xml file where the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html) is implemented, and the [coverage page](https://checkstyle.org/google-style.html#Google.27s_Java_Style_Checkstyle_Coverage) covers how each section of the guide is enforced and samples for each.

The first goal is to resolve all issues labeled [google style](https://github.com/checkstyle/checkstyle/labels/google%20style). The second is to reduce the "Blue Tick" rules on the coverage page. The third is to investigate issues in the Checkstyle tracker related to the Modules/Checks used in [google_checks.xml](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml). If the configuration reported in an issue matches the one in google_checks.xml, the issue qualifies for the project and is marked with the [google style](https://github.com/checkstyle/checkstyle/labels/google%20style) label. Alongside these, the [coverage page](https://checkstyle.org/google-style.html#Google.27s_Java_Style_Checkstyle_Coverage) is to be updated to match the latest version of the style guide, [Apr 09 2026](https://github.com/google/styleguide/commit/b5c8ecae344b5697210b9aa9a3ce08b608cea239)..

---

## What I Did During GSoC

**1. Resolving False Negatives in google_checks.xml**

A false negative means code that breaks the Google Java Style Guide but still passes validation with our configuration.

* *Blocks and statements:* [#11410](https://github.com/checkstyle/checkstyle/issues/11410) covered a `{` preceded by a block comment, which `LeftCurly` didn't flag. [#20963](https://github.com/checkstyle/checkstyle/issues/20963) covered a `{` with content after it on class, interface, annotation and record types, and on single-line switch blocks. [#17662](https://github.com/checkstyle/checkstyle/issues/17662) covered rework of `OneStatementPerLine` resolving false positives and false negatives.
* *Naming:* [#20563](https://github.com/checkstyle/checkstyle/issues/20563) covered type variable names such as `KEYT` and `VALUET`, which the config regex didn't flag.
* *Javadoc:* [#19724](https://github.com/checkstyle/checkstyle/issues/19724) fixed `JavadocMethod` being restricted to `public` while `MissingJavadocMethod` used `protected`, which meant bad Javadoc on protected methods went unreported. [#9806](https://github.com/checkstyle/checkstyle/issues/9806) covered summaries that weren't processed inside HTML tags.
* *Text blocks and annotations:* [#19707](https://github.com/checkstyle/checkstyle/issues/19707) covered `"""` preceded by `?` in a ternary, which went unflagged while the `:` branch was caught. [#20901](https://github.com/checkstyle/checkstyle/issues/20901) extended `AnnotationLocation` to `ANNOTATION_DEF`, `ANNOTATION_FIELD_DEF`, `ENUM_CONSTANT_DEF` and `PACKAGE_DEF`, added after comparing google-java-format's behaviour for each.
* *Whitespace:* [#17728](https://github.com/checkstyle/checkstyle/issues/17728) covered missing spacing between a type and a variable. [#17723](https://github.com/checkstyle/checkstyle/issues/17723) covered missing whitespace around `//` single line comments.

**2. Adding New Checks and Activating Them in google_checks.xml**

* [#17715](https://github.com/checkstyle/checkstyle/issues/17715) created and added `WhitespaceBeforeEmptyBody` for empty bodies, which the `allowEmptyXxxxxx` properties of `WhitespaceAround` skipped.
* [#18420](https://github.com/checkstyle/checkstyle/issues/18420) created the dedicated `GoogleMethodName` Check, and [#17841](https://github.com/checkstyle/checkstyle/issues/17841) added it to google_checks.xml.
* [#17695](https://github.com/checkstyle/checkstyle/issues/17695) created `MultilineCommentLeadingAsteriskPresence` for `*` on subsequent lines of `/* ... */` comments, and [#20995](https://github.com/checkstyle/checkstyle/issues/20995) activated it with an xpath suppression for native method bodies.
* [#7541](https://github.com/checkstyle/checkstyle/issues/7541) adds `GoogleRightCurly` to replace the `RightCurly` instance in google_checks.xml and remove the inlined suppression (in review).
* [#19968](https://github.com/checkstyle/checkstyle/issues/19968) created `AvoidModuleImport` for the `import module M;` form, which [#21445](https://github.com/checkstyle/checkstyle/issues/21445) then added to the config.

**3. Updating the Coverage Page and Adding New Rules**

At [#20936](https://github.com/checkstyle/checkstyle/issues/20936) the cached style guide page and its references were updated to the [latest Google Java Style Guide revision](https://github.com/google/styleguide/commit/b5c8ecae344b5697210b9aa9a3ce08b608cea239). New rules and missing samples were added in the child issues of [#21428](https://github.com/checkstyle/checkstyle/issues/21428).

**4. Resolving Issues in Modules and Docs Used by google_checks.xml**

Many defects that surface as style guide violations live in the modules or their documentation.

* *Crashes:* [#20720](https://github.com/checkstyle/checkstyle/issues/20720) fixed an NPE in `TextBlockGoogleStyleFormatting` and [#21609](https://github.com/checkstyle/checkstyle/issues/21609) fixed an NPE in `RightCurly`.
* *False positives:* [#21101](https://github.com/checkstyle/checkstyle/issues/21101) fixed `guava33_4_6` being rejected despite multipart version numbers being allowed. [#18842](https://github.com/checkstyle/checkstyle/issues/18842) fixed `/** {@return the customer ID} */` being reported as a forbidden summary fragment. [#3885](https://github.com/checkstyle/checkstyle/issues/3885) fixed a `FallThrough` warning on branches ending in an infinite loop.
* *Missing coverage:* [#20562](https://github.com/checkstyle/checkstyle/issues/20562) added `LocalFinalVariableName` to the config, to cover final local variables naming.
* *Docs:* [#6447](https://github.com/checkstyle/checkstyle/issues/6447) fixed `WhitespaceAround` docs not explaining that `{` parses as `SLIST` or `LCURLY`. [#16603](https://github.com/checkstyle/checkstyle/issues/16603) added descriptions of what blocks full coverage to blue-tick entries. [#19892](https://github.com/checkstyle/checkstyle/issues/19892) fixed `XdocsPagesTest` allowing only one config link per rule entry.

---

### Current Status and Future Work

All planned work has been merged except [#7541](https://github.com/checkstyle/checkstyle/issues/7541) (`GoogleRightCurly`), which is in review. The [coverage page](https://checkstyle.org/google-style.html) now reflects the [latest Google Java Style Guide revision](https://github.com/google/styleguide/commit/b5c8ecae344b5697210b9aa9a3ce08b608cea239), including the new rules and missing examples added this summer.

Several areas remain open for future work:

- **Coverage for `module-info.java`:** Checkstyle added support for module declarations at [#8240](https://github.com/checkstyle/checkstyle/issues/8240). The next step is covering them in google_checks.xml and updating the [coverage page](https://checkstyle.org/google-style.html), so module files are validated against the style guide like any other source file.
- **Remaining false negatives:**
  - [#18271](https://github.com/checkstyle/checkstyle/issues/18271) tracks Javadoc content that begins with a leading asterisk.
  - [#18273](https://github.com/checkstyle/checkstyle/issues/18273) tracks Javadoc blocks where the closing `*/` does not sit on its own line.

---

### Contributions

- All PRs/Issues related to the project can be found in the [GitHub project board](https://github.com/orgs/checkstyle/projects/14).
- All my contributions to Checkstyle can be found [here](https://github.com/checkstyle/checkstyle/pulls/Atharv3221).
- All issues opened by me can be found [here](https://github.com/checkstyle/checkstyle/issues/created_by/Atharv3221).

---

### What I Learned During GSoC

- **Designing and implementing new modules:** Creating new Checks meant starting from a style guide rule and working down to AST tokens, properties and edge cases. It taught me how Checkstyle's API works internally, and how to design a module that fits alongside existing ones rather than duplicating them.

- **Bug solving:** A defect could come from either the config or the Check itself, so each one started with tracing which it was. Working through code written by other contributors over many years taught me to read unfamiliar code carefully before changing it.

- **Code quality:** Checkstyle enforces 100% coverage with JaCoCo, mutation testing with PIT, and its own strict coding standards on every change. Working under these rules taught me to write code that doesn't just work, but follows the project's standards and is fully tested.

- **Decision making:** Many problems had more than one valid fix, from adjusting the config to extending an existing Check or creating a new one. Weighing these options taught me to pick the fix that stays correct as the codebase grows, not just the quickest one.

- **Project management:** Breaking large work into small, reviewable issues showed me how a big project stays easy to integrate and follow.

- **Communication:** Writing clear issue descriptions, explaining the reasoning behind a change in reviews, and asking precise questions taught me to communicate technical ideas concisely, especially in writing, where most open-source discussion happens.

- **Teamwork and collaboration:** Discussing approaches with mentors, handling review feedback and agreeing on priorities before writing code taught me how decisions in a large open-source project are made together rather than alone.

Overall, this experience gave me a much deeper understanding of how a mature open-source project is maintained, and the confidence to work on large, unfamiliar codebases.

---

### Acknowledgements

I would like to thank my mentors, [Roman Ivanov](https://github.com/romani), [Mauryan Kansara](https://github.com/Zopsss) and [Mohit Sharma](https://github.com/mohitsatr), for their guidance and reviews throughout this project. I learned a lot from working with them.
