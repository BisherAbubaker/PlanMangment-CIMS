# PlanMangment-CIMS
Assignment project code 
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Main.java to edit this template
 */
package planmanagementapp;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;
import java.util.Collections;

// 1. Base Plan class
class Plan {
    protected int planId;
    protected String planName;
    protected double price;

    public Plan(int planId, String planName, double price) {
        this.planId = planId;
        this.planName = planName;
        this.price = price;
    }

    public int getPlanId() {
        return planId;
    }

    @Override
    public String toString() {
        return "ID: " + planId + ", Name: " + planName + ", Price: " + price;
    }
}

// 2. SpecialPlan class inherits Plan
class SpecialPlan extends Plan {
    private String benefits;

    public SpecialPlan(int planId, String planName, double price, String benefits) {
        super(planId, planName, price);
        this.benefits = benefits;
    }

    public void setBenefits(String benefits) {
        this.benefits = benefits;
    }

    @Override
    public String toString() {
        return super.toString() + ", Benefits: " + benefits;
    }
}

// 3. PlanManager class to store and manage plans
class PlanManager {
    private ArrayList<Plan> plans = new ArrayList<>();

    public void addPlan(Plan plan) {
        plans.add(plan);
        Collections.sort(plans, (p1, p2) -> Integer.compare(p1.getPlanId(), p2.getPlanId()));
    }

    public ArrayList<Plan> getPlans() {
        return plans;
    }

    public Plan searchPlan(int id) {
        int index = binarySearch(id);
        if (index != -1) return plans.get(index);
        return null;
    }

    public boolean updatePlan(int id, String name, double price, String benefits, boolean isSpecial) {
        int index = binarySearch(id);
        if (index != -1) {
            Plan p = plans.get(index);
            p.planName = name;
            p.price = price;
            if (p instanceof SpecialPlan && isSpecial) {
                ((SpecialPlan) p).setBenefits(benefits);
            }
            return true;
        }
        return false;
    }

    public boolean deletePlan(int id) {
        int index = binarySearch(id);
        if (index != -1) {
            plans.remove(index);
            return true;
        }
        return false;
    }

    // Binary search on sorted list
    private int binarySearch(int id) {
        int left = 0, right = plans.size() - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (plans.get(mid).getPlanId() == id) return mid;
            if (plans.get(mid).getPlanId() < id) left = mid + 1;
            else right = mid - 1;
        }
        return -1;
    }
}

// GUI class
public class PlanManagementGUI extends JFrame {
    private PlanManager manager = new PlanManager();
    private DefaultListModel<String> listModel = new DefaultListModel<>();
    private JList<String> planList = new JList<>(listModel);

    public PlanManagementGUI() {
        setTitle("CIMS Plan Management");
        setSize(600, 400);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // Input panel
        JPanel inputPanel = new JPanel(new GridLayout(6, 2, 5, 5));
        JTextField idField = new JTextField();
        JTextField nameField = new JTextField();
        JTextField priceField = new JTextField();
        JTextField benefitsField = new JTextField();
        JCheckBox specialBox = new JCheckBox("Special Plan?");
        JButton addBtn = new JButton("Add");
        JButton updateBtn = new JButton("Update");
        JButton deleteBtn = new JButton("Delete");
        JButton searchBtn = new JButton("Search");

        inputPanel.add(new JLabel("Plan ID:"));
        inputPanel.add(idField);
        inputPanel.add(new JLabel("Plan Name:"));
        inputPanel.add(nameField);
        inputPanel.add(new JLabel("Price:"));
        inputPanel.add(priceField);
        inputPanel.add(new JLabel("Benefits:"));
        inputPanel.add(benefitsField);
        inputPanel.add(specialBox);
        inputPanel.add(new JLabel()); // Empty
        inputPanel.add(addBtn);
        inputPanel.add(updateBtn);

        JPanel bottomPanel = new JPanel();
        bottomPanel.add(deleteBtn);
        bottomPanel.add(searchBtn);

        add(inputPanel, BorderLayout.NORTH);
        add(new JScrollPane(planList), BorderLayout.CENTER);
        add(bottomPanel, BorderLayout.SOUTH);

        // Event handlers
        addBtn.addActionListener(e -> {
            try {
                int id = Integer.parseInt(idField.getText());
                String name = nameField.getText();
                double price = Double.parseDouble(priceField.getText());
                if (specialBox.isSelected()) {
                    String benefits = benefitsField.getText();
                    manager.addPlan(new SpecialPlan(id, name, price, benefits));
                } else {
                    manager.addPlan(new Plan(id, name, price));
                }
                refreshList();
                JOptionPane.showMessageDialog(this, "Plan added!");
            } catch (Exception ex) {
                JOptionPane.showMessageDialog(this, "Invalid input!");
            }
        });

        updateBtn.addActionListener(e -> {
            try {
                int id = Integer.parseInt(idField.getText());
                boolean updated = manager.updatePlan(id, nameField.getText(),
                        Double.parseDouble(priceField.getText()), benefitsField.getText(), specialBox.isSelected());
                if (updated) {
                    refreshList();
                    JOptionPane.showMessageDialog(this, "Plan updated!");
                } else {
                    JOptionPane.showMessageDialog(this, "Plan not found!");
                }
            } catch (Exception ex) {
                JOptionPane.showMessageDialog(this, "Error updating plan!");
            }
        });

        deleteBtn.addActionListener(e -> {
            try {
                int id = Integer.parseInt(idField.getText());
                if (manager.deletePlan(id)) {
                    refreshList();
                    JOptionPane.showMessageDialog(this, "Plan deleted!");
                } else {
                    JOptionPane.showMessageDialog(this, "Plan not found!");
                }
            } catch (Exception ex) {
                JOptionPane.showMessageDialog(this, "Error deleting plan!");
            }
        });

        searchBtn.addActionListener(e -> {
            try {
                int id = Integer.parseInt(idField.getText());
                Plan p = manager.searchPlan(id);
                if (p != null) JOptionPane.showMessageDialog(this, "Found: " + p);
                else JOptionPane.showMessageDialog(this, "Plan not found!");
            } catch (Exception ex) {
                JOptionPane.showMessageDialog(this, "Error searching plan!");
            }
        });

        setVisible(true);
    }

    private void refreshList() {
        listModel.clear();
        for (Plan p : manager.getPlans()) {
            listModel.addElement(p.toString());
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(PlanManagementGUI::new);
    }
}
