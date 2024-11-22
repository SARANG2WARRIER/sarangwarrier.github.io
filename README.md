<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Verilog Code Generator</title>
    <style>
        /* Basic Reset */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        /* Body Styling with Subtle Neon Greenish Theme */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #0d0d0d2d; /* Dark background for neon effect */
            color: #00ff99; /* Neon greenish text color */
            text-align: center;
            padding: 20px;
            background-image: url('grad2.jpg'); /* Add the URL of your image here */
            background-size: cover; /* Cover the full screen */
            background-position: center; /* Center the image */
            background-attachment: fixed; /* Make the background image fixed while scrolling */
        }

        /* Subtle Neon Glow Effect */
        h1, h2 {
            text-shadow:
                0 0 2px #00ff99,
                0 0 3px #00ff99,
                0 0 7px #00ff99;
            margin-bottom: 20px;
        }

        p, label, button, input, a {
            text-shadow: 0 0 2px #00ff99;
        }

        /* Page Sections */
        .page {
            display: none;
            padding: 20px;
        }

        #welcomePage {
            display: block; /* Show welcome page initially */
            position: relative;
            z-index: 1;
        }

        /* Buttons Styling with Subtle Neon Glow */
        .button-container {
            margin: 20px 0;
        }

        .button-container button {
            padding: 12px 24px;
            margin: 10px;
            font-size: 16px;
            border: 2px solid #00ff99;
            border-radius: 6px;
            background-color: transparent;
            color: #00ff99;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .button-container button::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: rgba(0, 255, 153, 0.05);
            transform: rotate(45deg);
            transition: all 0.5s ease;
        }

        .button-container button:hover {
            background-color: rgba(0, 255, 153, 0.05);
            box-shadow: 0 0 10px #00ff99, 0 0 15px #00ff99, 0 0 20px #00ff99;
        }

        /* Input Form Styling with Subtle Neon Glow */
        #inputForm input[type="text"] {
            padding: 10px;
            font-size: 16px;
            width: 250px;
            margin-bottom: 20px;
            border: 2px solid #00ff99;
            border-radius: 4px;
            background-color: transparent;
            color: #00ff99;
            transition: border-color 0.3s ease;
        }

        #inputForm input[type="text"]::placeholder {
            color: rgba(0, 255, 153, 0.7);
        }

        #inputForm input[type="text"]:focus {
            border-color: #66ffcc;
            outline: none;
            box-shadow: 0 0 5px #66ffcc, 0 0 10px #66ffcc;
        }

        #inputForm button {
            padding: 10px 20px;
            font-size: 16px;
            border: 2px solid #00ff99;
            background-color: transparent;
            color: #00ff99;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        #inputForm button::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: rgba(0, 255, 153, 0.05);
            transform: rotate(45deg);
            transition: all 0.5s ease;
        }

        #inputForm button:hover {
            background-color: rgba(0, 255, 153, 0.05);
            box-shadow: 0 0 10px #00ff99, 0 0 15px #00ff99, 0 0 20px #00ff99;
        }

        /* Verilog Output Styling with Subtle Neon Effect */
        #verilogOutputContainer {
            margin-top: 20px;
            text-align: left;
            width: 90%;
            max-width: 900px;
            margin-left: auto;
            margin-right: auto;
        }

        #verilogOutput {
            white-space: pre-wrap; /* Preserve formatting */
            background-color: #1e1e1e;
            color: #00ff99;
            padding: 20px;
            border-radius: 6px;
            overflow-x: auto;
            font-family: 'Courier New', Courier, monospace;
            box-shadow: 0 0 5px #00ff99, 0 0 10px #00ff99;
            max-height: 500px;
            overflow-y: auto;
        }

        /* Download and Copy Buttons Styling */
        .output-buttons {
            margin-top: 15px;
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        .output-buttons button, .output-buttons a {
            padding: 10px 20px;
            font-size: 16px;
            border: 2px solid #00ff99;
            background-color: transparent;
            color: #00ff99;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            text-decoration: none;
        }

        .output-buttons button::before,
        .output-buttons a::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: rgba(0, 255, 153, 0.05);
            transform: rotate(45deg);
            transition: all 0.5s ease;
        }

        .output-buttons button:hover,
        .output-buttons a:hover {
            background-color: rgba(0, 255, 153, 0.05);
            box-shadow: 0 0 10px #00ff99, 0 0 15px #00ff99, 0 0 20px #00ff99;
        }

        /* TASK Selection Page Styling */
        #taskSelectionPage {
            position: relative;
            z-index: 1;
        }

        /* New TASK 2 Options Page Styling */
        #task2OptionsPage {
            position: relative;
            z-index: 1;
        }

        /* New TASK 1 Options Page Styling */
        #task1OptionsPage {
            position: relative;
            z-index: 1;
        }

        /* New TASK 4 Options Page Styling */
        #task4OptionsPage {
            position: relative;
            z-index: 1;
        }

        /* Responsive Design */
        @media (max-width: 600px) {
            #inputForm input[type="text"] {
                width: 90%;
            }

            #verilogOutputContainer {
                width: 100%;
            }

            .output-buttons {
                flex-direction: column;
            }

            .output-buttons button, .output-buttons a {
                width: 100%;
            }
        }
    </style>
</head>
<body>

    <!-- Welcome Page -->
    <div id="welcomePage" class="page">
        <h1>Welcome to VeriGen: Verilog Code Generator</h1>
        <h2 style="margin-bottom:10px;">created by Sarang Warrier (23BEC0168) and Yuvan Shankar Raja(23BEC0317)</h2>
        <p>Initializing...</p>
    </div>

    <!-- TASK Selection Page -->
    <div id="taskSelectionPage" class="page">
        <h2>Select a Task</h2>
        <div class="button-container">
            <button id="task1Btn">TASK 1</button>
            <button id="task2Btn">TASK 2</button>
            <button id="task3Btn">TASK 3</button>
            <button id="task4Btn">TASK 4</button>
            <button id="task5Btn">TASK 5</button>
        </div>
        <div class="button-container">
            <button id="verilogCodeBtn">Verilog Code using Registration Number</button>
        </div>
        <div class="button-container">
            <p>It is suggested to open the below links from desktop browser :</p>
        </div>
        <div class="button-container">
            <button onclick="window.location.href='https://colab.research.google.com/drive/1OeLCjXxiy5DWTnMTnE3pFJEnFeNe-_Xz#scrollTo=E6p1KqmXml0d';">
                Verilog Code for assigned BCD</button>
        </div>
        <div class="button-container">
            <button onclick="window.location.href='https://colab.research.google.com/drive/1MLmBC3ZzMmUPELOZ4fKDbpaoFVZd2NQr?usp=sharing';">
                Verilog Code for assigned Excess N Counter</button>
        </div>
        <div class="button-container">
            <button onclick="window.location.href='https://colab.research.google.com/drive/1dDO2S-bTS7h6vBlQQg8UEYtApfg6cgaf?usp=sharing';">
                Verilog Code for assigned Mod N Counter</button>
        </div>
            
    </div>

    <!-- TASK 1 Options Page -->
    <div id="task1OptionsPage" class="page">
        <h2>TASK 1: Click on Verilog Code Link</h2>
        <!-- Back Button -->
        <div class="button-container">
            <button id="backBtn_task1OptionsPage">Back</button>
        </div>
        <!-- TASK 1 Buttons -->
        <div class="button-container">
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1pvI';">
                HA using DFL
            </button>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1pvK';">
                HA using BL
            </button>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1pvJ';">
                HA using SL
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1pvM';">
                FA using DFL
            </button>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1pvS';">
                FA using BL
            </button>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1pvN';">
                FA using SL
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1pvz';">
                HS using DFL
            </button>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1pvF';">
                HS using BL
            </button>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1pvB';">
                HS using SL
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1pwT';">
                FS using DFL
            </button>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1px0';">
                FS using BL
            </button>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1pwV';">
                FS using SL
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1jpe';">  
                4 bit Adder SL        
            </button>   <!--DONE-->
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1oW6';">  
                4 bit Adder/Sub        
            </button>   <!--DONE-->
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1oW1';">
                4 bit Multiplier     
            </button>     <!--DONE-->
        </div>
    </div>

    <!-- TASK 2 Options Page -->
    <div id="task2OptionsPage" class="page">
        <h2>TASK 2: Click on Verilog Code Link</h2>
        <!-- Back Button -->
        <div class="button-container">
            <button id="backBtn_task2OptionsPage">Back</button>
        </div>
        <!-- TASK 2 Buttons -->
        <div class="button-container">
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1joL';" id="task2DataFlowBtn">
                Registration number using Dataflow Level Verilog Code
            </button>
            <br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1joI';" id="task2BehavioralBtn">
                Registration number using Behavioral Level Verilog Code
            </button>
            <br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1joy';" id="task2StructuralBtn">
                Registration number using Structural Level Verilog Code
            </button>
            <br>
        </div>
    </div>

    <!-- TASK 3 Options Page -->
    <div id="task3OptionsPage" class="page">
        <h2>TASK 3: Click on Verilog Code Link</h2>
        <!-- Back Button -->
        <div class="button-container">
            <button id="backBtn_task3OptionsPage">Back</button>
        </div>
        <!-- TASK 3 Buttons -->
        <div class="button-container">
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1gVM';">
                SL 64:1 MUX
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1gVS';">
                DFL 32:1 MUX
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1gVO';">
                BL 4:1 MUX using logical equality
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/a/8bkw ';">
                2:1 Mux using Conditional Operator
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1hrv';">
                Boolean Expression using 16:1 Multiplexer
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1hxI';">
                Boolean Expression using 4:16 Decoder Verilog code
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1hrB';">
                HA using 4:1 MUX
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1hrF';">
                FA using 8:1 MUX
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1hs0';">
                HS using 2:1 MUX
            </button><br>
        </div>
    </div>

    <!-- TASK 4 Options Page -->
    <div id="task4OptionsPage" class="page">
        <h2>TASK 4: Click on Verilog Code Link</h2>
        <!-- Back Button -->
        <div class="button-container">
            <button id="backBtn_task4OptionsPage">Back</button>
        </div>
        <!-- TASK 4 Buttons -->
        <div class="button-container">
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1gcS';">
                1 bit ALU
            </button>
            <button onclick="window.location.href='https://www.jdoodle.com/a/88vy';">
                4 bit ALU
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1ges';">
                8 bit ALU
            </button>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1gUc';">
                32 bit ALU
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1j5Q';">
                4 bit Booth Multiplier
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/a/8bdO';">
                1 bit Comparator
            </button>
            
        </div>
    </div>

    <!-- TASK 5 Options Page -->
    <div id="task5OptionsPage" class="page">
        <h2>TASK 5: Click on Verilog Code Link</h2>
        <!-- Back Button -->
        <div class="button-container">
            <button id="backBtn_task5OptionsPage">Back</button>
        </div>
        <!-- TASK 5 Buttons -->
        <div class="button-container">
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1p0h';">
                Bi-directional Shift register
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1jxu';">
                Up Down Gray Counter 
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1p9j';">
                Asynchronous 4-bit Up/Down Counter
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1p0o';">
                Synchronous 4-bit Up/Down counter
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1p0t';">
                Ring Counter
            </button>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1p18';">
                Johnson Counter
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1p1d';">
                Up/Down Random Counter Synchronous
            </button><br>
            <button onclick="window.location.href='https://www.jdoodle.com/ia/1p1f';">
                Up/Down Random Counter Asynchronous
            </button><br>

        </div>
    </div>

    <!-- Model Selection Page -->
    <div id="modelSelectionPage" class="page">
        <h2>Select a Verilog Model</h2>
        <!-- Back Button -->
        <div class="button-container">
            <button id="backBtn_modelSelectionPage">Back</button>
        </div>
        <!-- Model Selection Buttons -->
        <div class="button-container">
            <button id="dataFlowBtn">DATA FLOW MODEL</button>
            <button id="behavioralBtn">BEHAVIORAL MODEL</button>
            <button id="structuralBtn">STRUCTURAL MODEL</button>
        </div>
    </div>

    <!-- Input Form Page -->
    <div id="inputFormPage" class="page">
        <h2 id="formTitle">Enter Your Registration Number</h2>
        <!-- Back Button -->
        <div class="button-container">
            <button id="backBtn_inputFormPage">Back</button>
        </div>
        <!-- Input Form -->
        <form id="inputForm">
            <input type="text" id="regNumber" placeholder="e.g., 25BCE0001" required />
            <br />
            <button type="submit">Generate Verilog Code</button>
        </form>
    </div>

    <!-- Verilog Output Page -->
    <div id="verilogOutputContainer" class="page">
        <h2>Generated Verilog Code</h2>
        <!-- Back Button -->
        <div class="button-container">
            <button id="backBtn_verilogOutputContainer">Back</button>
        </div>
        <!-- Verilog Output -->
        <pre id="verilogOutput"></pre>
        <!-- Download and Copy Buttons -->
        <div class="output-buttons">
            <a id="downloadBtn" href="#" download="verilog_code.v">Download Verilog Code</a>
            <button id="copyBtn">Copy Code</button>
        </div>
    </div>

    <!-- JavaScript -->
    <script>
        // Page Elements
        const welcomePage = document.getElementById("welcomePage");
        const taskSelectionPage = document.getElementById("taskSelectionPage");
        const task1OptionsPage = document.getElementById("task1OptionsPage"); // TASK 1 Page
        const task2OptionsPage = document.getElementById("task2OptionsPage"); // TASK 2 Page
        const task3OptionsPage = document.getElementById("task3OptionsPage"); // TASK 3 Page
        const task4OptionsPage = document.getElementById("task4OptionsPage"); // TASK 4 Page
        const task5OptionsPage = document.getElementById("task5OptionsPage"); // TASK 5 Page
        const modelSelectionPage = document.getElementById("modelSelectionPage");
        const inputFormPage = document.getElementById("inputFormPage");
        const verilogOutputContainer = document.getElementById("verilogOutputContainer");
        const formTitle = document.getElementById("formTitle");
        const inputForm = document.getElementById("inputForm");
        const regNumberInput = document.getElementById("regNumber");
        const verilogOutput = document.getElementById("verilogOutput");
        const downloadBtn = document.getElementById("downloadBtn");
        const copyBtn = document.getElementById("copyBtn");
        const verilogCodeBtn = document.getElementById("verilogCodeBtn");

        // Back Buttons
        const backBtn_task1OptionsPage = document.getElementById("backBtn_task1OptionsPage");
        const backBtn_task2OptionsPage = document.getElementById("backBtn_task2OptionsPage");
        const backBtn_task3OptionsPage = document.getElementById("backBtn_task3OptionsPage");
        const backBtn_task4OptionsPage = document.getElementById("backBtn_task4OptionsPage");
        const backBtn_task5OptionsPage = document.getElementById("backBtn_task5OptionsPage");
        const backBtn_modelSelectionPage = document.getElementById("backBtn_modelSelectionPage");
        const backBtn_inputFormPage = document.getElementById("backBtn_inputFormPage");
        const backBtn_verilogOutputContainer = document.getElementById("backBtn_verilogOutputContainer");

        let selectedModel = ""; // To store the selected model type

        // Transition from Welcome Page to TASK Selection Page after 4 seconds
        setTimeout(() => {
            welcomePage.style.display = "none";
            taskSelectionPage.style.display = "block";
        }, 4000); // 4000 milliseconds = 4 seconds

        // Event Listeners for TASK Selection Buttons (TASK 1 to TASK 5)
        document.getElementById("task1Btn").addEventListener("click", () => {
            taskSelectionPage.style.display = "none";
            task1OptionsPage.style.display = "block";
        });

        document.getElementById("task2Btn").addEventListener("click", () => {
            taskSelectionPage.style.display = "none";
            task2OptionsPage.style.display = "block";
        });

        document.getElementById("task3Btn").addEventListener("click", () => {
            taskSelectionPage.style.display = "none";
            task3OptionsPage.style.display = "block";
        });

        document.getElementById("task4Btn").addEventListener("click", () => {
            taskSelectionPage.style.display = "none";
            task4OptionsPage.style.display = "block";
        });

        document.getElementById("task5Btn").addEventListener("click", () => {
            taskSelectionPage.style.display = "none";
            task5OptionsPage.style.display = "block";
        });

        // Event Listener for "Verilog codes using Registration Number" Button
        verilogCodeBtn.addEventListener("click", () => {
            taskSelectionPage.style.display = "none";
            modelSelectionPage.style.display = "block";
        });

        // Event Listeners for Model Selection Buttons
        document.getElementById("dataFlowBtn").addEventListener("click", () => {
            selectedModel = "Data Flow";
            proceedToInputForm();
        });

        document.getElementById("behavioralBtn").addEventListener("click", () => {
            selectedModel = "Behavioral";
            proceedToInputForm();
        });

        document.getElementById("structuralBtn").addEventListener("click", () => {
            selectedModel = "Structural";
            proceedToInputForm();
        });


        // Event Listeners for Back Buttons
        backBtn_task1OptionsPage.addEventListener("click", () => {
            task1OptionsPage.style.display = "none";
            taskSelectionPage.style.display = "block";
        });

        backBtn_task2OptionsPage.addEventListener("click", () => {
            task2OptionsPage.style.display = "none";
            taskSelectionPage.style.display = "block";
        });

        backBtn_task3OptionsPage.addEventListener("click", () => {
            task3OptionsPage.style.display = "none";
            taskSelectionPage.style.display = "block";
        });

        backBtn_task4OptionsPage.addEventListener("click", () => {
            task4OptionsPage.style.display = "none";
            taskSelectionPage.style.display = "block";
        });

        backBtn_task5OptionsPage.addEventListener("click", () => {
            task5OptionsPage.style.display = "none";
            taskSelectionPage.style.display = "block";
        });

        backBtn_modelSelectionPage.addEventListener("click", () => {
            modelSelectionPage.style.display = "none";
            taskSelectionPage.style.display = "block";
        });

        backBtn_inputFormPage.addEventListener("click", () => {
            inputFormPage.style.display = "none";
            modelSelectionPage.style.display = "block";
        });

        backBtn_verilogOutputContainer.addEventListener("click", () => {
            verilogOutputContainer.style.display = "none";
            inputFormPage.style.display = "block";
        });

        // Function to Show Input Form Page
        function proceedToInputForm() {
            // Hide all relevant pages before showing input form
            task1OptionsPage.style.display = "none";
            task2OptionsPage.style.display = "none";
            task3OptionsPage.style.display = "none";
            task4OptionsPage.style.display = "none";
            task5OptionsPage.style.display = "none";
            modelSelectionPage.style.display = "none";
            inputFormPage.style.display = "block";
            formTitle.textContent = `Enter Your Registration Number for ${selectedModel} Model`;
        }

        // Event Listener for Form Submission
        inputForm.addEventListener("submit", (e) => {
            e.preventDefault(); // Prevent form from submitting
            const regNumber = regNumberInput.value.trim().toUpperCase();

            if (!regNumber) {
                alert("Please enter a valid registration number.");
                return;
            }

            // Generate Verilog Code Based on Selected Model
            if (selectedModel === "Data Flow") {
                const verilogCode = generateDataFlowVerilog(regNumber);
                displayVerilogCode(verilogCode);
            } else if (selectedModel === "Behavioral") {
                const verilogCode = generateBehavioralVerilog(regNumber);
                displayVerilogCode(verilogCode);
            } else if (selectedModel === "Structural") {
                const verilogCode = generateStructuralVerilog(regNumber);
                displayVerilogCode(verilogCode);
            }
        });

        // Function to Display Verilog Code and Show Download & Copy Options
        function displayVerilogCode(code) {
            inputFormPage.style.display = "none";
            verilogOutputContainer.style.display = "block";
            verilogOutput.textContent = code;

            // Prepare Download Link
            const blob = new Blob([code], { type: 'text/plain' });
            const url = URL.createObjectURL(blob);
            downloadBtn.href = url;
            downloadBtn.download = selectedModel === "Data Flow" ? "data_flow_model.v" :
                                   selectedModel === "Behavioral" ? "behavioral_model.v" :
                                   "structural_model.v";
        }

        // Function to Copy Code to Clipboard
        copyBtn.addEventListener("click", () => {
            const code = verilogOutput.textContent;
            navigator.clipboard.writeText(code).then(() => {
                // Provide feedback to the user
                copyBtn.textContent = "Copied!";
                setTimeout(() => {
                    copyBtn.textContent = "Copy Code";
                }, 2000);
            }).catch(err => {
                alert("Failed to copy code: ", err);
            });
        });

        // Function to Determine Minterms Based on Registration Number
        function determineMinterms(regNumber) {
            const minterms = [];
            const mapping = {
                'A': [10],
                'B': [11],
                'C': [12],
                'D': [13],
                'E': [14],
                'F': [15]

                // Add more mappings as needed
            };

            // Iterate over mapping keys
            for (const [key, values] of Object.entries(mapping)) {
                if (regNumber.includes(key)) {
                    minterms.push(...values);
                }
            }

            // Check for digits and add corresponding minterms
            for (let i = 0; i < 16; i++) {
                if (regNumber.includes(i.toString())) {
                    minterms.push(i);
                }
            }

            // Remove duplicates and ensure minterms are within 0-15
            return [...new Set(minterms)].filter(m => m >= 0 && m < 16);
        }

        // Function to Generate Data Flow Verilog Code
        function generateDataFlowVerilog(regNumber) {
            const minterms = determineMinterms(regNumber);
            if (minterms.length === 0) {
                alert("No valid minterms found based on the registration number.");
                return "// No Verilog code generated due to invalid minterms.";
            }

            let verilogCode = `module logic_function(\n`;
            verilogCode += `    input A, B, C, D,\n`;
            verilogCode += `    output F\n`;
            verilogCode += `);\n\n`;
            verilogCode += `    assign F = \n        `;

            // Create logic expression based on minterms
            const terms = minterms.map(m => {
                const binary = m.toString(2).padStart(4, '0');
                let term = '(';

                for (let j = 0; j < binary.length; j++) {
                    term += binary[j] === '1' ? `${String.fromCharCode(65 + j)}` : `~${String.fromCharCode(65 + j)}`;
                    if (j < binary.length - 1) {
                        term += ' & ';
                    }
                }

                term += ')';
                return term;
            }).join(' | \n        ');

            verilogCode += terms + ";\n\nendmodule\n\n";

            // Testbench Module
            verilogCode += `module testbench;\n`;
            verilogCode += `    reg A, B, C, D;\n`;
            verilogCode += `    wire F;\n`;
            verilogCode += `    logic_function lf(A, B, C, D, F);\n\n`;
            verilogCode += `    initial begin\n`;
            verilogCode += `        $monitor("Time: %0t | A: %b | B: %b | C: %b | D: %b | F: %b", $time, A, B, C, D, F);\n\n`;

            // Generate all combinations
            for (let i = 0; i < 16; i++) {
                const binary = i.toString(2).padStart(4, '0');
                verilogCode += `        A = ${binary[0]}; B = ${binary[1]}; C = ${binary[2]}; D = ${binary[3]}; #10;\n`;
            }

            verilogCode += `        $finish;\n`;
            verilogCode += `    end\n`;
            verilogCode += `endmodule\n`;

            return verilogCode;
        }

        // Function to Generate Behavioral Verilog Code
        function generateBehavioralVerilog(regNumber) {
            const minterms = determineMinterms(regNumber);
            if (minterms.length === 0) {
                alert("No valid minterms found based on the registration number.");
                return "// No Verilog code generated due to invalid minterms.";
            }

            let verilogCode = `module behavioral_logic(\n`;
            verilogCode += `    input A, B, C, D,\n`;
            verilogCode += `    output reg F\n`;
            verilogCode += `);\n\n`;
            verilogCode += `    always @(A or B or C or D) begin\n`;
            verilogCode += `        case ({A, B, C, D})\n`;

            // Add cases for minterms
            minterms.forEach(m => {
                const binary = m.toString(2).padStart(4, '0');
                verilogCode += `            4'b${binary}: F = 1; // Minterm ${m}\n`;
            });

            verilogCode += `            default: F = 0;\n`;
            verilogCode += `        endcase\n`;
            verilogCode += `    end\n\nendmodule\n\n`;

            // Testbench Module
            verilogCode += `module testbench;\n`;
            verilogCode += `    reg A, B, C, D;\n`;
            verilogCode += `    wire F;\n`;
            verilogCode += `    behavioral_logic bl(A, B, C, D, F);\n\n`;
            verilogCode += `    initial begin\n`;
            verilogCode += `        $monitor("Time: %0t | A: %b | B: %b | C: %b | D: %b | F: %b", $time, A, B, C, D, F);\n\n`;

            // Generate all combinations
            for (let i = 0; i < 16; i++) {
                const binary = i.toString(2).padStart(4, '0');
                verilogCode += `        A = ${binary[0]}; B = ${binary[1]}; C = ${binary[2]}; D = ${binary[3]}; #10;\n`;
            }

            verilogCode += `        $finish;\n`;
            verilogCode += `    end\n`;
            verilogCode += `endmodule\n`;

            return verilogCode;
        }

        // Function to Generate Structural Verilog Code
        function generateStructuralVerilog(regNumber) {
            const minterms = determineMinterms(regNumber);
            if (minterms.length === 0) {
                alert("No valid minterms found based on the registration number.");
                return "// No Verilog code generated due to invalid minterms.";
            }

            // Define gate_logic module only once
            let verilogCode = `module gate_logic(\n`;
            verilogCode += `    input A, B, C, D,\n`;
            verilogCode += `    output F\n`;
            verilogCode += `);\n\n`;

            // Generate F using gates
            verilogCode += `    wire `;
            minterms.forEach((m, index) => {
                verilogCode += `m${m}${index < minterms.length - 1 ? ', ' : ''}`;
            });
            verilogCode += `;\n\n`;

            // Define AND gates for each minterm
            minterms.forEach(m => {
                const binary = m.toString(2).padStart(4, '0');
                verilogCode += `    and AND_${m}(m${m}, ${binary[0] === '1' ? 'A' : '~A'}, ${binary[1] === '1' ? 'B' : '~B'}, ${binary[2] === '1' ? 'C' : '~C'}, ${binary[3] === '1' ? 'D' : '~D'});\n`;
            });

            verilogCode += `\n    or OR_F(F, ${minterms.map(m => `m${m}`).join(', ')});\n\nendmodule\n\n`;

            // Testbench Module
            verilogCode += `module testbench;\n`;
            verilogCode += `    reg A, B, C, D;\n`;
            verilogCode += `    wire F;\n`;
            verilogCode += `    gate_logic gl(A, B, C, D, F);\n\n`;
            verilogCode += `    initial begin\n`;
            verilogCode += `        $monitor("Time: %0t | A: %b | B: %b | C: %b | D: %b | F: %b", $time, A, B, C, D, F);\n\n`;

            // Generate all combinations
            for (let i = 0; i < 16; i++) {
                const binary = i.toString(2).padStart(4, '0');
                verilogCode += `        A = ${binary[0]}; B = ${binary[1]}; C = ${binary[2]}; D = ${binary[3]}; #10;\n`;
            }

            verilogCode += `        $finish;\n`;
            verilogCode += `    end\n`;
            verilogCode += `endmodule\n`;

            return verilogCode;
        }
    </script>
</body>
</html>
