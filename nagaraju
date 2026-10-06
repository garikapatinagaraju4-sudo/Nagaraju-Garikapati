import streamlit as st

# =====================================================================
# HYDRAULIC TURBINE CALCULATOR (STREAMLIT VERSION)
# =====================================================================

# Global Constants
DENSITY_WATER = 1000.0  # Density of water in kg/m³
GRAVITY = 9.81          # Acceleration due to gravity in m/s²

def calculate_hydraulic_power(discharge, head):
    """Calculates the total potential fluid power input (Hydraulic Power)."""
    return DENSITY_WATER * GRAVITY * discharge * head

def calculate_shaft_power(hydraulic_power, overall_efficiency):
    """Calculates the mechanical power output at the turbine shaft."""
    return hydraulic_power * overall_efficiency

def calculate_overall_efficiency(shaft_power, hydraulic_power):
    """Calculates the overall efficiency of the turbine system."""
    if hydraulic_power == 0:
        return 0.0
    return shaft_power / hydraulic_power

def calculate_required_discharge(target_shaft_power, head, overall_efficiency):
    """Calculates the discharge flow rate required to yield a target shaft power."""
    denominator = DENSITY_WATER * GRAVITY * head * overall_efficiency
    if denominator == 0:
        return 0.0
    return target_shaft_power / denominator

# --- Streamlit UI App Layout ---
st.set_page_config(page_title="Hydraulic Turbine Calculator", page_icon="💧", layout="wide")

st.title("💧 Hydraulic Turbine Calculator")
st.markdown("Determine hydraulic power, shaft power, overall efficiency, and required flow rates interactively.")

# Sidebar Configuration for Inputs
st.sidebar.header("🔧 Turbine Input Parameters")

q = st.sidebar.number_input(
    "Actual Discharge/Flow Rate (Q) [m³/s]", 
    min_value=0.0, max_value=500.0, value=2.5, step=0.1, format="%.2f"
)
h = st.sidebar.number_input(
    "Net Operating Head (H) [meters]", 
    min_value=0.0, max_value=1000.0, value=40.0, step=1.0, format="%.1f"
)
eff_input = st.sidebar.slider(
    "Expected Overall Efficiency (%)", 
    min_value=0.0, max_value=100.0, value=85.0, step=0.5
)

# Convert percentage slider value to a decimal fraction
efficiency = eff_input / 100.0

# Layout splits calculations into two distinct visual areas
col1, col2 = st.columns(2)

with col1:
    st.subheader("📊 Output Analysis")
    
    # Execution logic
    hydraulic_power_w = calculate_hydraulic_power(q, h)
    shaft_power_w = calculate_shaft_power(hydraulic_power_w, efficiency)
    verified_efficiency = calculate_overall_efficiency(shaft_power_w, hydraulic_power_w)
    
    # Display calculated outputs cleanly as interactive metric cards
    st.metric(label="Hydraulic Power Available", value=f"{hydraulic_power_w / 1000.0:.2f} kW")
    st.metric(label="Shaft Power Produced", value=f"{shaft_power_w / 1000.0:.2f} kW")
    st.metric(label="Calculated System Efficiency", value=f"{verified_efficiency * 100.0:.1f}%")

with col2:
    st.subheader("🎯 Required Flow Planning")
    
    target_kw = st.number_input(
        "Enter Target Shaft Power Output [kW]:", 
        min_value=0.0, max_value=100000.0, value=800.0, step=10.0
    )
    
    target_w = target_kw * 1000.0  # Convert to Watts for underlying calculation logic
    required_q = calculate_required_discharge(target_w, h, efficiency)
    
    # Alert message displaying the requirement dynamically
    st.info(f"💡 **Required Discharge:** To achieve a target output of **{target_kw:.2f} kW**, the fluid system requires a continuous volumetric discharge of **{required_q:.3f} m³/s**.")
