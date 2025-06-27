# documentation
This repo hosts official documentation for the Artifact Virtual project.


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Abstract Topological Banner</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            background-color: #000000; /* AMOLED black */
        }
        .banner-background {
            /* Using multiple linear and radial gradients at different angles and sizes
               to create a more organic, interconnected, and less grid-like feel. */
            background-image:
                /* Subtle diagonal lines */
                linear-gradient(45deg, rgba(255,255,255,0.03) 1px, transparent 1px),
                linear-gradient(-45deg, rgba(255,255,255,0.03) 1px, transparent 1px),
                /* Softer, larger horizontal/vertical "waves" */
                linear-gradient(90deg, rgba(255,255,255,0.02) 1px, transparent 1px),
                linear-gradient(0deg, rgba(255,255,255,0.02) 1px, transparent 1px),
                /* Radial "nodes" for interconnection points */
                radial-gradient(circle at 25% 25%, rgba(255,255,255,0.05) 1px, transparent 2px),
                radial-gradient(circle at 75% 75%, rgba(255,255,255,0.05) 1px, transparent 2px),
                radial-gradient(circle at 25% 75%, rgba(255,255,255,0.05) 1px, transparent 2px),
                radial-gradient(circle at 75% 25%, rgba(255,255,255,0.05) 1px, transparent 2px);
            
            background-size:
                80px 80px, /* Diagonal base */
                80px 80px,
                120px 120px, /* Larger horizontal/vertical base */
                120px 120px,
                100px 100px, /* Radial nodes base */
                100px 100px,
                100px 100px,
                100px 100px;
            
            background-position:
                0 0, 0 0, 
                0 0, 0 0,
                0 0, 0 0,
                0 0, 0 0;
            
            opacity: 0.8; /* Subtle transparency */
            animation: moveTopologicalLines 120s linear infinite; /* Very slow, subtle animation */
        }

        @keyframes moveTopologicalLines {
            to {
                /* Different movement for different layers to create complexity */
                background-position:
                    4000px 4000px, /* Diagonal move */
                    -4000px 4000px,
                    0 6000px, /* Vertical move */
                    6000px 0, /* Horizontal move */
                    1000px 1000px, /* Radial move */
                    -1000px -1000px,
                    1000px -1000px,
                    -1000px 1000px;
            }
        }
    </style>
</head>
<body class="flex items-center justify-center min-h-screen">
    <div class="w-full h-72 overflow-hidden relative bg-gray-950">
        <div class="absolute inset-0 banner-background"></div>
        <div class="absolute inset-0 flex items-center justify-center">
            <h1 class="text-white text-5xl font-light tracking-wider opacity-90">
                ARTIFACT VIRTUAL
            </h1>
        </div>
    </div>
</body>
</html>

