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
                   round(input$fert / .46, 1),  # Urea typically has 46% nitrogen
                   round(input$fert / .34, 1))  # Ammonium nitrate typically has 34% nitrogen
    
    totalN <- rate * input$area  # Calculate total nitrogen needed
    output$result <- renderText({
      paste("To fertilize", input$area, "acres at", 
            input$fert, "pounds of N per acre using", input$fertilizerType, "you will need", totalN, "lbs of", input$fertilizerType)
    })
  })
}

# Run the application 
shinyApp(ui = ui, server = server)
