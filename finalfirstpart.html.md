```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Parameter List with Search and Clear</title>
    <style>
        * {
            padding: 0px;
            margin: 0px;
            box-sizing: border-box;
        }

        button {
            cursor: pointer;
        }

        :root {
            --black: #222;
            --gray: #ccc;
            --sky-blue: rgb(33, 122, 226);
        }

        .filter-container {
            overflow-x: auto;
            white-space: nowrap;
            padding: 16px 0;
            margin-bottom: 16px;
            -webkit-overflow-scrolling: touch; /* For smoother scrolling on iOS */
        }

        .filter {
            border: 1px solid var(--gray);
            border-radius: 8px;
            width: 200px;
            display: inline-block;
            margin-right: 16px;
            vertical-align: top;
        }

        .filter__heading {
            color: var(--black);
            font-size: 0.75rem;
            font-weight: 600;
            padding: 12px;
            border-bottom: 1px solid var(--gray);
        }

        .filter__options {
            list-style: none;
            padding: 0;
            margin: 0;
            overflow-y: auto;
            max-height: 100px;
        }

        .filter__option {
            font-size: 0.75rem;
            padding: 8px 12px;
        }

        .filter__option > button {
            color: inherit;
            width: 100%;
            text-align: left;
            border: none;
            background: none;
        }

        .filter__option.active {
            background: var(--sky-blue);
            color: white;
        }

        .filter__actions {
            padding: 8px 12px;
            border-top: 1px solid var(--gray);
        }

        .filter__actions__button {
            border: none;
            background: none;
            font-size: 0.75rem;
            cursor: pointer;
        }
    </style>
</head>

<body>
    <div class="filter-container" id="filterContainer"></div>

    <div id="result"></div>

    <script>
        const parameterOptions = {
            "voltage": ["50V", "100V", "200V", "500V"],
            "current": ["3A", "5A", "10A", "20A"],
            "waveform": ["Sine", "Square", "Triangle", "Sawtooth"],
            "controller_temp": ["50°C", "60°C", "70°C", "80°C"],
            "motor_temp": ["40°C", "50°C", "60°C", "70°C"],
            "fault_indication": ["Error 1", "Error 2", "Error 3", "Error 4"],
            "regenerative_braking": ["Enabled", "Disabled"],
            "communication": ["Ethernet", "WiFi", "Bluetooth", "RS485"],
            "motor_type": ["AC", "DC", "Stepper", "Servo"]
        };

        const renderFilter = (containerId, parameter, options) => {
            const container = document.getElementById(containerId);

            const parameterContainer = document.createElement("div");
            parameterContainer.classList.add("filter");
            container.appendChild(parameterContainer);

            const heading = document.createElement("div");
            heading.classList.add("filter__heading");
            heading.textContent = parameter;
            parameterContainer.appendChild(heading);

            const optionsList = document.createElement("ul");
            optionsList.classList.add("filter__options");
            parameterContainer.appendChild(optionsList);

            options.forEach(option => {
                optionsList.appendChild(createFilterOption(option));
            });

            const clearBtn = document.createElement("button");
            clearBtn.textContent = "Clear Selection";
            clearBtn.classList.add("filter__actions__button");
            clearBtn.addEventListener("click", () => {
                clearSelection(optionsList);
                renderResults();
            });
            parameterContainer.appendChild(clearBtn);
        };

        const createFilterOption = (label) => {
            const listEle = document.createElement("li");
            listEle.classList.add("filter__option");
            const buttonEle = document.createElement("button");
            buttonEle.textContent = label;
            buttonEle.addEventListener("click", (e) => filterOptionCallback(e, label));
            listEle.appendChild(buttonEle);
            return listEle;
        };

        let selectedOptions = [];
        function filterOptionCallback(event, option) {
            if (selectedOptions.includes(option)) {
                event.target.parentElement.classList.remove("active");
                selectedOptions = selectedOptions.filter((op) => op != option);
            } else {
                selectedOptions.push(option);
                event.target.parentElement.classList.add("active");
            }
            renderResults();
        }

        const clearSelection = (optionsList) => {
            selectedOptions = [];
            optionsList.querySelectorAll('.filter__option.active').forEach((element) => {
                element.classList.remove("active");
            });
        };

        const renderResults = () => {
            const resultEle = document.getElementById("result");
            resultEle.textContent = selectedOptions.join(", ");
        };

        const filterContainer = document.getElementById("filterContainer");
        for (const parameter in parameterOptions) {
            if (parameterOptions.hasOwnProperty(parameter)) {
                renderFilter("filterContainer", parameter, parameterOptions[parameter]);
            }
        }
    </script>
</body>
</html>
```
![image](https://github.com/Mogana004/work-repo/assets/92911280/e6b11ff0-1f5e-401c-9a7f-64f0895539c6)
