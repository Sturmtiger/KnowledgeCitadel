#SOLID #SRP

>A class should have one and only one reason to change. - Robert Martin
## Consequences of Violating SRP
Requirements may change over time.
The more responsibilities a class has, the more often you'll need to modify it.
If your class implements multiple responsibilities, they are not independent, and tend to be coupled.

Having multiple responsibilities brings another issue: whenever you need to change your class, you might also need to update all its dependencies, even if they are not directly affected by your change.
The dependencies may use only one of the class's responsibilities, but you need to update all of them.

The class has too many dependencies because of having multiple responsibilities.

In the end, you need to modify your class and its dependencies more often, and each modification is more complicated because multiple responsibilities are coupled within one class.
It also makes the class's usage in its dependencies more confusing.
## Pros of SRP adherence
Classes that have only one responsibility are much easier to explain, understand and implement.
There's much less room for bugs, development becomes faster, and testing such a class is easier.
## Code Examples
### ❌ SRP Violation
```python
class Report:
    def __init__(self, title: str, content: str) -> None:
        self.title = title
        self.content = content

    def format_html(self) -> str:
        return f"<html><head><title>{self.title}</title></head><body>{self.content}</body></html>"

    def save_to_file(self, filename: str) -> None:
	    with open(filename, "w") as file:
            file.write(self.format_html())


if __name__ == "__main__":
    report = Report("Expenses", "Expenses details")
    report.save_to_file()
```
####  What's wrong?
`Report` has the following responsibilities:
1. Representing the data of a report ✅
2. Handling formatting (HTML) ⚠️
3. Handling persistence (saving to file) ⚠️
Whenever the requirements of any of these responsibilities change, the class must be modified.
### ✅ SRP-Compliant version
Each responsibility is delegated to a separate class.
```python
class Report:
    def __init__(self, title: str, content: str) -> None:
        self.title = title
        self.content = content


class HtmlFormatter:
    def format(report: Report) -> str:
        return f"<html><head><title>{report.title}</title></head><body>{report.content}</body></html>"


class FileSaver:
    def save(self, html_content: str, filename: str) -> None:
	    with open(filename, "w") as file:
            file.write(html_content)


if __name__ == "__main__":
    report = Report("Expenses", "Expenses details")
    formatter = HtmlFormatter()
    saver = FileSaver()

    html_content = formatter.format(report)
    saver.save(html_content, "report.html")
```

Now, each class clearly adheres to one responsibility:
- `Report`: holds report data ✅
- `HTMLFormatter`: formats the report to HTML ✅
- `FileSaver`: handles saving the formatted report to a file ✅
### ❌ Overusing SRP
Let's use HTML formatting logic to demonstrate how SRP can be taken to an extreme by creating separate classes for trivial tasks that could, and should, live in a single class.
```python
class HtmlTitleFormatter:
    def format(title: str) -> str:
        return f"<title>{title}</title>"


class HtmlBodyFormatter:
    def format(content: str) -> str:
        return f"<body>{content}</body>"


class HtmlReportAssembler:
    def assemble(title: str, body: str) -> str:
        return f"<html><head>{title}</head>{body}</html>"


if __name__ == "__main__":
    title_formatter = HtmlTitleFormatter()
    body_formatter = HtmlBodyFormatter()
    report_assembler = HtmlReportAssembler()

    title_html = title_formatter.format(report.title)
    body_html = body_formatter.format(report.content)
    report_html = HTMLReportAssembler.assemble(title_html, body_html)

```
This design demonstrates excessive complexity and overhead without meaningful benefit. Having multiple tiny classes, each responsible for trivial functionality, makes the code harder to understand, maintain, and read.

SRP is important but don't oversimplify your code by taking SRP to an extreme. Keep it balanced!