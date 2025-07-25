library(shiny)

# Define the user interface
ui <- fluidPage(
  titlePanel("Nitrogen Fertilizer Rate Calculator"),
  
  sidebarLayout(
    sidebarPanel(
      selectInput("fertilizerType",
                  "Select Fertilizer Type:",
                  choices = c("Urea", "Ammonium Nitrate"),
                  selected = "Urea"),
      numericInput("area", 
                   "Area (in acres):", 
                   value = 1, 
                   min = 0),
      numericInput("fert", 
                   "Nitrogen (in pounds):", 
                   value = 1, 
                   min = 0),
      actionButton("calculate", "Calculate Rate")
    ),
    
    mainPanel(
      textOutput("result")
    )
  )
)

# Define server logic
server <- function(input, output) {
  observeEvent(input$calculate, {
    rate <- ifelse(input$fertilizerType == "Urea",
                   round(input$fert / .46, 1),
                   round(input$fert / .34, 1))
    
    totalN <- rate * input$area  # Calculate total nitrogen needed
    output$result <- renderText({
      paste("To fertilize", input$area, "acres at", 
            input$fert, "pounds of N per acre using", input$fertilizerType, "you will need", totalN, "lbs of", input$fertilizerType)
    })
  })
}

# Run the application 
shinyApp(ui = ui, server = server)




<img width="630" height="494" alt="image" src="https://github.com/user-attachments/assets/73a46ebd-ea7c-448e-8351-401b56b04735" />

